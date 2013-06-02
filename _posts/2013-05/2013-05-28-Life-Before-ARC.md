---
layout: post
title: Life-Before-ARC
categories:
- Programming
tags:
- iOS
---

[**Pro Multithreading and Memory Management for iOS and OS X**:with ARC, Grand Central Dispatch, and Blocks](http://book.douban.com/subject/10536953/)

这本书很不错，推荐给大家。讲述了ARC的原理，GCD、Block使用以及其底层实现机制。

第一章讲的是手动管理内存的相关知识。



**一.引用计数内存管理概述**  
Objective-C使用“引用计数”来做内存管理。

1）你拥有你创建的对象的所有权，此时的引用计数是1.  
2）你可以使用retain来获得对象的所有权，此时引用计数加1.  
3）当你用完对象的时候，你必须释放掉所有权，此时引用计数减1.  
4）你不得释放并不拥有所有权的对象.  
5) 当一个对象的引用计数为0时，其dealloc方法会被调用.  
6）当一个对象被销毁后，如果还通过先前的引用进行操作，那就可能引发carsh或者不可预料的异常。  

下图是上面的action对应的Objective-C方法：  
![](http://farm9.staticflickr.com/8403/8867314869_9e8010763e.jpg)

用了创建对象的方法名称都有一定的规律，方法为以下这些或者以这些开头的方法。

.alloc   
.new  
.copy   
.mutableCopy  

在我们自己的代码中创建对象也需要遵照同样的规则。     

如果一个函数返回一个新创建的对象 比如 [NSMutableArray array] 这样的,调用者对返回的对象并没有拥有权,但是在返回的时候新建的对象又不能在返回前释放,那该如何实现呢? 下面是一个解决此类问题的例子。

    - (id)object{
    id obj = [[NSObject alloc] init];//此时拥有对象的所有权。
    [obj autorelease];//释放对象的所有权,这个时候对象并不会立即销毁,因为此时对象的所有者为当前活动的autoreleasePool。
    return obj;
    }

通过调用autorelease,可以创建并返回一个对象，但是不拥有其所有权。Autorelease提供了一种机制，当对象的生命周期结束时候，它会释放掉对象。

下图是release和autorelease的区别:    
![](http://farm6.staticflickr.com/5339/8867321717_2257ec65c7.jpg)

**二.引用计数的实现原理**

alloc, retain, release, dealloc都是NSObject的方法，位于Foundation Framework,并没有被苹果开源，虽然Core Foundation是开源的，但是并不能看到苹果是如何实现的全貌。所以，我们可以参看GNUstep是如何实现的。      

GNUsteop是Cocoa Framework的一个兼容实现，虽然不可能和苹果的实现一模一样，但是它的实现方式也有助于我们了解苹果是如何实现的。

**alloc方法**  

通过一些简化，alloc的实现可以如下表示：
    struct obj_layout {   
      NSUInteger retained;  
    }; 
      
    + (id) alloc   
    {   
       int size = sizeof(struct obj_layout) + size_of_the_object;   
       struct obj_layou *p = (struct obj_layout *)calloc(1,size);   
       return (id)(p + 1);  
    } 

下图是alloc方法返回的对象示例：

![](http://farm6.staticflickr.com/5329/8867321873_45e0061b1e.jpg)

**retain方法**  

    - (id) retain
    {
       NSIncrementExtraRefCount(self);
       return self;
    }
    
    inline void
    NSIncrementExtraRefCount(id anObject)
    {
      if (((struct obj_layout *)anObject)[-1].retained == UINT_MAX - 1)
      [NSException raise: NSInternalInconsistencyException
      format: @"NSIncrementExtraRefCount() asked to increment too far"];
    
     ((struct obj_layout *)anObject)[-1].retained++;
    }


**release方法**  

    - (void) release
    {
       if(NSDecrementExtraRefCountWasZero(self))
        [self dealloc];
    }
    
    BOOL
    NSDecrementExtraRefCountWasZero(id anObject)
    {
         if(((struct obj_layout *)anObject)[-1].retained == 0) {
           return YES;
          } else {       
          ((struct obj_layout *)anObject)[-1].retained--;
          return NO;
         }
    }

**dealloc方法**  

    - (void) dealloc
    {
       NSDeallocateObject (self);
    }
    
    inline void
    NSDeallocateObject(id anObject)
    {
         struct obj_layout *o = &((struct obj_layout *)anObject)[-1];
         free(o);
    }


**Apple的alloc,retain, release, dealloc的实现**

alloc的调用堆栈如下：  

    +alloc
    +allocWithZone:
    class_createInstance
    calloc


NSObject调用allocWithZone之后，调用class_createInstance在其内部调用calloc分配内存。


那retainCount,retain,release是如何实现的呢? 以下为各个方法的调用栈


    -retainCount 
    __CFDoExternRefOperation
    CFBasicHashGetCountOfKey
    
    -retain 
    __CFDoExternRefOperation 
    CFBasicHashAddValue
    
    -release 
    __CFDoExternRefOperation 
    CFBasicHashRemoveValue
    // 当CFBasicHashRemoveValue返回0的时候，dealloc方法会被调用
可以发现retainCount,retain,release使用到了同一个CF函数__CFDoExternRefOperation,此函数是开源的，代码在下面的文件中 http://www.opensource.apple.com/source/CF/CF-635.21/CFRuntime.c. 


    CF_EXPORT uintptr_t __CFDoExternRefOperation(uintptr_t op, id obj) {
    if (nil == obj) HALT;
    uintptr_t idx = EXTERN_TABLE_IDX(obj);
    uintptr_t disguised = DISGUISE(obj);
    CFSpinLock_t *lock = &__NSRetainCounters[idx].lock;
    CFBasicHashRef table = __NSRetainCounters[idx].table;
    uintptr_t count;
    switch (op) {
    case 300:   // increment
    case 350:   // increment, no event
    __CFSpinLock(lock);
    	CFBasicHashAddValue(table, disguised, disguised);
    __CFSpinUnlock(lock);
    if (__CFOASafe && op != 350) __CFRecordAllocationEvent(__kCFObjectRetainedEvent, obj, 0, 0, NULL);
    return (uintptr_t)obj;
    case 400:   // decrement
    if (__CFOASafe) __CFRecordAllocationEvent(__kCFObjectReleasedEvent, obj, 0, 0, NULL);
    case 450:   // decrement, no event
    __CFSpinLock(lock);
    count = (uintptr_t)CFBasicHashRemoveValue(table, disguised);
    __CFSpinUnlock(lock);
    return 0 == count;
    case 500:
    __CFSpinLock(lock);
    count = (uintptr_t)CFBasicHashGetCountOfKey(table, disguised);
    __CFSpinUnlock(lock);
    return count;
    }
    return 0;
    }
    
    
对象的引用计数的信息保存在一个哈希表中.见图：  
![](http://farm6.staticflickr.com/5464/8867320507_7239a1e7b3_b.jpg)

**小结**
留个问题，GNUstep的实现和Apple的实现，各自的优势是什么呢？




**三. 自动释放池的实现原理**

（待续...）































