---
layout: post  
title: Monkey Patching iOS with Swizzling  
categories:  
- 其他  
tags:
- Jekyll
---

**1 什么是Monkey-patching**  
"Monkey-patching is extending or modifying the behavior of code at runtime without changing its original source code. "
在Objective-C中，Monkey-patching可以通过category来实现。  
之前的一篇文章**[Store Data in a Category](http://wufawei.com/2013/05/Store-Data-in-a-Category/)**介绍了如何在Category中保存数据。

通过Category可以定义新的方法，也可以覆盖已有的方法。
[Monkey-Patching iOS with Objective-C Categories Part I: Simple Extensions and Overrides](http://blog.carbonfive.com/2012/01/23/monkey-patching-ios-with-objective-c-categories-part-1-simple-extensions-and-overrides/) 介绍了覆盖已有方法的危险，不推荐使用这种方法。
比如说：  
a)其他framework，SDK可能期望的是原来被覆盖的方法。  
b）如果多个category都覆盖了同样的方法，但是加载顺序并不能确定。

**2 解决方法：Swizzling**

"Swizzling is simply exchanging the implementation of two of a class’ methods so that when a message is sent using the original selector of one it actually goes to the other"

通过Objective-C的运行时函数，可以实现swizzling，代码如下:

    SEL firstMethodSelector = @selector(firstMethod);
    SEL secondMethodSelector = @selector(secondMethod);
    Method firstMethod = class_getInstanceMethod(self, firstMethodSelector);
    Method secondMethod = class_getInstanceMethod(self, secondMethodSelector);
     
    BOOL methodAdded = class_addMethod([self class],
       firstMethodSelector,
       method_getImplementation(secondMethod),
       method_getTypeEncoding(secondMethod));
      
    if (methodAdded) {
    class_replaceMethod([self class], 
      secondMethodSelector, 
      method_getImplementation(firstMethod),
      method_getTypeEncoding(firstMethod));
    } else {
      method_exchangeImplementations(firstMethod, secondMethod);
    }

  a)首先，我们建立要swizzling的方法的selector,在这里是firstMethod和secondMethod.
  b)获取这些selector执行的方法。
  c)首先我们尝试把第二个方法的实现加在第一个方法的selector下，这么做的原因是防止第一个方法不存在。
  d)如果c)中，方法成功被加进去，这就意味着第一个方法是空的，那第二个方法需要点东西，那我们就简单的用第一个方法的（空）的实现和第二个方法简单替换下。
  e)如果我们替换失败，说明第一个方法存在，那我们就直接交换两者的实现就可以了。

一般来说，我们很少会交换2个已有方法，通常会进入一个新的方法，然后和已有的方法交换。任何调用原来的方法都会调用新的实现。

**3 Swizzle on Load**  
Objective-C的Runtime会在load category的时候，调用类方法load。这是实现swizzle的好时机，通过dispatch_once来保证只执行一次。


    @implementation SomeClass (CategoryName)
     
    + (void)load {
      static dispatch_once_t onceToken;
      dispatch_once(&onceToken, ^{
       SEL viewWillAppearSelector = @selector(viewWillAppear:);
       SEL tourGuideWillAppearSelector =  @selector(tourGuideWillAppear:);
       Method originalMethod = class_getInstanceMethod(self, viewWillAppearSelector);
       Method newMethod = class_getInstanceMethod(self, tourGuideWillAppearSelector);
    
       BOOL methodAdded = class_addMethod([self class],
      viewWillAppearSelector,
      method_getImplementation(newMethod),
      method_getTypeEncoding(newMethod));
     
       if (methodAdded) {
          class_replaceMethod([self class], 
          tourGuideWillAppearSelector, 
          method_getImplementation(originalMethod),
          method_getTypeEncoding(originalMethod));
       } else {
          method_exchangeImplementations(originalMethod, newMethod);
       }
      });
    }

**4 NSObject category**

通过在NSObject中加上一个类方法，可以避免在每个category中写重复的代码

    #import <Foundation/Foundation.h>
     
    @interface NSObject (Swizzle)
     
    + (void)swizzleInstanceSelector:(SEL)originalSelector 
                    withNewSelector:(SEL)newSelector;
     
    @end
    
    #import "NSObject+Swizzle.h"
    #import <objc/runtime.h>
     
    @implementation NSObject (Swizzle)
     
    + (void) swizzleInstanceSelector:(SEL)originalSelector 
                     withNewSelector:(SEL)newSelector
    {
      Method originalMethod = class_getInstanceMethod(self, originalSelector);
      Method newMethod = class_getInstanceMethod(self, newSelector);
     
      BOOL methodAdded = class_addMethod([self class],
                                         originalSelector,
                                         method_getImplementation(newMethod),
                                         method_getTypeEncoding(newMethod));
      
      if (methodAdded) {
            class_replaceMethod([self class], 
                               newSelector, 
                               method_getImplementation(originalMethod),
                               method_getTypeEncoding(originalMethod));
      } else {
            method_exchangeImplementations(originalMethod, newMethod);
      }
    }
     
    @end

**5 Example:UIKit Main Thread Guard**

"This is a guard that tracks down UIKit access on threads other than main. This snippet is taken from the commercial iOS PDF framework http://pspdfkit.com, but relicensed under MIT. Works because a lot of calls internally call setNeedsDisplay or setNeedsLayout. Won't catch everything, but it's very lightweight and usually does the job. You might need to add DEBUG=1 to your preprocessor macros if you haven't done that already."

下面是通过Monkey-patch，swizzling来swizzling setNeedsDisplay, setNeedsLayout，setNeedsDisplayInRect等来判断对UIKit的访问是不是在主线程。

代码如下：

    #import <objc/runtime.h>
    #import <objc/message.h>
    #import <QuartzCore/QuartzCore.h>
     
    #define PROPERTY(propName) NSStringFromSelector(@selector(propName))
     
    // A better assert. NSAssert is too runtime dependant, and assert() doesn't log.
    // http://www.mikeash.com/pyblog/friday-qa-2013-05-03-proper-use-of-asserts.html
    // Accepts both:
    // - PSPDFAssert(x > 0);
    // - PSPDFAssert(y > 3, @"Bad value for y");
    #define PSPDFAssert(expression, ...) \
    do { if(!(expression)) { \
    NSLog(@"%@", [NSString stringWithFormat: @"Assertion failure: %s in %s on line %s:%d. %@", #expression, __PRETTY_FUNCTION__, __FILE__, __LINE__, [NSString stringWithFormat:@"" __VA_ARGS__]]); \
    abort(); }} while(0)
     
    // You should only use this in debug builds. It doesn't use private API, but I wouldn't ship it.
    #ifdef DEBUG
     
    static void PSPDFSwizzleMethod(Class c, SEL orig, SEL new) {
    	Method origMethod = class_getInstanceMethod(c, orig);
    	Method newMethod = class_getInstanceMethod(c, new);
    	if (class_addMethod(c, orig, method_getImplementation(newMethod), method_getTypeEncoding(newMethod))) {
    	class_replaceMethod(c, new, method_getImplementation(origMethod), method_getTypeEncoding(origMethod));
    	}else {
			method_exchangeImplementations(origMethod, newMethod);
    	}
    }
     
    void PSPDFReplaceMethod(Class c, SEL orig, SEL newSel, IMP impl) {
    	Method method = class_getInstanceMethod(c, orig);
    	if (!class_addMethod(c, newSel, impl, method_getTypeEncoding(method))) {
    	PSPDFLogError(@"Failed to add method: %@ on %@", NSStringFromSelector(newSel), c);
    	}else PSPDFSwizzleMethod(c, orig, newSel);
    }
     
    ///////////////////////////////////////////////////////////////////////////////////////////
    #pragma mark - Tracks down calls to UIKit from a Thread other than Main
     
    static void PSPDFAssertIfNotMainThread(void) {
    	PSPDFAssert([NSThread isMainThread], @"\nERROR: All calls to UIKit need to happen on the main thread. You have a bug in your code. Use dispatch_async(dispatch_get_main_queue(), ^{ ... }); if you're unsure what thread you're in.\n\nBreak on PSPDFAssertIfNotMainThread to find out where.\n\nStacktrace: %@", [NSThread callStackSymbols]);
    }
     
    // This installs a small guard that checks for the most common threading-errors in UIKit.
    // This won't really slow down performance but still only is compiled in DEBUG versions of PSPDFKit.
    // @note No private API is used here.
    __attribute__((constructor)) static void PSPDFUIKitMainThreadGuard(void) {
    	@autoreleasepool {
    	for (NSString *selector in @[PROPERTY(setNeedsLayout), PROPERTY(setNeedsDisplay), PROPERTY(setNeedsDisplayInRect:)]) {
    	SEL newSelector = NSSelectorFromString([NSString stringWithFormat:@"pspdf_%@", selector]);
    	if ([selector hasSuffix:@":"]) {
    	PSPDFReplaceMethod(UIView.class, NSSelectorFromString(selector), newSelector, imp_implementationWithBlock(^(UIView *_self, CGRect r) {
    	PSPDFAssertIfNotMainThread();
    	((void ( *)(id, SEL, CGRect))objc_msgSend)(_self, newSelector, r);
    	}));
    	}else {
    	PSPDFReplaceMethod(UIView.class, NSSelectorFromString(selector), newSelector, imp_implementationWithBlock(^(UIView *_self) {
    	PSPDFAssertIfNotMainThread();
    	((void ( *)(id, SEL))objc_msgSend)(_self, newSelector);
    	}));
    	}
    	}
    	}
    }
     

    #endif


更多参见：  
1）[Monkey-Patching iOS with Objective-C Categories Part III: Swizzling](http://blog.carbonfive.com/2013/02/20/monkey-patching-ios-with-objective-c-categories-part-iii-swizzling/)
2）<https://gist.github.com/steipete/5664345>