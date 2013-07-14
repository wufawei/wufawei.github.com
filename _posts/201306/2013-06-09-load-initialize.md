---
layout: post  
title: Objective-C类初始化:load与initialize  
categories:  
- Programming  
tags:
- iOS
---

在C语言中，main()是程序最早开始启动和初始化的地方。
在Objective-C中，application，documents，user interface都有自己初始化的地方，所以，在main()中只是简单的调用
NSApplicationMain。    
**Objective-C中第一个初始化的地方是 +(void)load**.   任何class都能有一个＋（void)load。


##1 ＋load

+load方法在类被加载到系统时调用，这发生在一个很早的时刻, +load的方法的调用顺序为:


1. 链接到的框架初始化(包括+load)  
2. 当前镜像(image)的+load  
3. 当前镜像的C++静态初始化和C/C++ __attribute__(constructor)构造函数   
4. 链接到你的框架初始化。  

下面可以看到load被加载的时机：
![](http://lh5.ggpht.com/mattxg/R98tQp0bsiI/AAAAAAAAAF4/zHhz3_etL9o/s800/startupMain.png)



**+load方法调用的时候尚未有自动释放池**，**所以如果+load中的内容使用到了自动释放池，则代码需要用@autoreleasepool{}包含。
另外，对应****Category中的+load方法并不会覆盖类自身中的+load方法**，**两个+load方法都会调用，而且Category的+load方法在类自身的+load方法之后调用。**



代码例子：

    @implementation LoadClass
    
    
    + (void)load
    {
    	NSLog(@"####loading");
    }
    
    
    __attribute__((constructor)) staticvoid ConstructorLoad(void)
    {
    	NSLog(@"ConstructorLoad ");
    }
    
    @end
    
    
    @implementation LoadClass (LoadClass_category)
    
    + (void)load
    {
    	NSLog(@"####LoadClass_category loading ");
    }
    
    @end
    
    
    int main(int argc, char *argv[])
    {
    	@autoreleasepool {
       
    	@autoreleasepool {
    	NSLog(@"main Start");
    	}
    	returnUIApplicationMain(argc, argv, nil, NSStringFromClass([AppDelegateclass]));
    	}
    }
    
**输出**

    ####loading
    ####LoadClass_category loading
    ConstructorLoad
    main Start

2 +initialize

+initialize方法在一个比较安全的环境中调用，比+load更适合放入初始化代码。+initialize方法在类首次收到消息时调用，若此类一直没有使用，则+initialize就不会调用。
与+load方法类似的时，在向子类发送initialize消息前，总是会先向父类发送initialize消息，如果父类尚未调用+initialize方法则调用

Category中的+initialize方法会覆盖类本身的+initialize方法。如果父类中实现了+initialize方法，而子类中没有重写此方法，则子类初始化时就会使用父类的方法。

代码：

    #import
    
    @interface AbstractBase : NSObject
    @end
    
    @implementation AbstractBase
    
    + (void)initialize
    {
    NSLog(@"%@ initialize in ",self);
    }
    @end
    
    @interface SubClasss : AbstractBase
    @end
    
    @implementation SubClasss
    @end
    
    int main()
    {
    @autoreleasepool {
    NSLog(@"main Start");
    [SubClasss class];
    NSLog(@"main End");
       
    }
    return0;
    }

    **其输出为:**

    main Start
    AbstractBase initialize
    SubClasss initialize
    main End

如果不想子类使用父类的+initialize方法，则父类的+initialize方法可以这样写

    + (void)initialize
    {
       if(self == [ParentClass class])
       {
         ... init..
       }
    }
    















1) The App Launch Sequence on iOS
<http://oleb.net/blog/2011/06/app-launch-sequence-ios/>

2) <http://www.friday.com/bbum/2009/09/06/iniailize-can-be-executed-multiple-times-load-not-so-much/>       

3) Objective-C类初始化:load与initialize  
<http://www.winddisk.com/2012/08/19/objective-c-class-load-initialize/>

4) Friday Q&A 2009-05-22: Objective-C Class Loading and Initialization
<http://www.mikeash.com/pyblog/friday-qa-2009-05-22-objective-c-class-loading-and-initialization.html>