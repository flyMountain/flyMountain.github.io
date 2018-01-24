---
layout: post
title: "Android MVC MVP"
date: 2017-9-10
description: "Android，模式"
tag: 模式
---

本文是区分MVP MVC 区别 优劣势

### MVC
MVC 有称Model View Controller 是软件的常用架构之一  这里View指的就是布局文件  Model指的是业务逻辑以及实体模型 Controllor 对应于Activity。简单的一句话概括就是Controllor控制操作model层的数据，然后结果返回给View层展示  
![](/images/mvc/1C3D2B1B36FCF9E280C4EFF678F25974.jpg)

具体到Android项目中MVC是怎样执行的那？从上面可知MVC的各个组成部分代表Android项目中的文件，当我们按下一个按键（Button）去执行下载任务，按键在View层中（布局文件）然后通过Controller层 Activity button.setOnClickListener()这个函数 然后再去调用Model层中的下载程序  
在这里我们已经有MVC可为什么还需要MVP模式那？从上面的分析我们可以知道view层就是一个xml文件，如果你想动态的改变一个背景图，改变一个图标的颜色 这时候都必须通过Activity实现，这就造成了Activity的代码会变的冗长，难以维护。还有一个重要原因就是在上图中我们可以知道Model和View层是相互的，可以知道MVC的耦合性太高 不利于大型项目  

这里就产生了MVP模式，MVC作为MVP的演化解决了MVC的两个缺点，MVP模式中M指的是Model
V指的是View P指的是Presenter    
* View 对应于Activity，负责View的绘制以及与用户交互
* Model 依然是业务逻辑和实体模型
* Presenter 负责完成View于Model间的交互  
![](/images/mvc/6179CD938B4CF9E221E80831E0972B38.jpg)


从图中就可以看出，最明显的差别就是view层和model层不再相互可知，完全的解耦，取而代之的presenter层充当了桥梁的作用，用于操作view层发出的事件传递到presenter层中，presenter层去操作model层，并且将数据返回给view层，整个过程中view层和model层完全没有联系。看到这里大家可能会问，虽然view层和model层解耦了，但是view层和presenter层不是耦合在一起了吗？其实不是的，对于view层和presenter层的通信，我们是可以通过接口实现的，具体的意思就是说我们的activity，fragment可以去实现实现定义好的接口，而在对应的presenter中通过接口调用方法。不仅如此，我们还可以编写测试用的View，模拟用户的各种操作，从而实现对Presenter的测试。这就解决了MVC模式维护难的问题  

例子网上有好多 我自己写的就不好意思往上放了。。。。。。。


