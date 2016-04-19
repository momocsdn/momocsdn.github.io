---
layout: post
title:  "Activity基础总结"
date:   2013-05-01 11:23:32
categories: jekyll update
---
本篇文章主要是总结一下Activity的一些基础知识，主要包括：启动模式和生命周期两部分，关于其他和Activity相关的一些知识，将在以后的其它章节中进行总结。

下面首先看一下`思维导图` ：
![Activity基础](http://7xt310.com2.z0.glb.clouddn.com/Activity%E5%9F%BA%E7%A1%80.png)


## 启动模式

 * standard：默认的启动模式，系统不关心这个活动在该APP的返回栈里面是否存在，每次启动都会创建该活动的一个新的实例，哪怕该活动已经处于栈顶了，再次启动该活动，依然会创建新的实例

 * singleTop：假如当前栈顶不是该Activity，则不管该返回栈中是否存在，都会创建一个新的实例；假如该Activity已经处于该返回栈的顶部，则不会再次去创建一个新的实例，而是去执行一个叫onNewIntent()的方法。

 * singleTask：假如当前栈顶不是该Activity，则如果返回栈里面已经存在该Activity，则将会把该Activity上面的所有Activity出栈，并让该Activity处于栈顶；如果该返回栈里面不存在，则直接创建一个新的实例；假如当前Activity存在并处于栈顶，则不会再次创建该Activity，而会执行onNewIntent()方法

 * singleInstance：主要是在多个APP共享一个Activity的时候才会使用，这种模式下会有一个单独的返回栈来管理该活动，当点击返回键的时候，当APP的栈 中所有的Activity都销毁之后，才会去销毁共享模式下的Activity



## 生命周期

 * 返回栈：所有的Activity创建好之后，都是保存在一个栈里面的，方便Activity的管理

 * 活动状态

    1. 运行状态：当Activity位于栈顶时，就进入了运行状态

    2. 暂停状态：当Activity不再处于栈顶，而且仍然可见，此时Activity就进入了暂停状态；eg.对话框形式的Activity半透明，或者只是占了半个屏幕

    3. 停止状态：当Activity不再处于栈顶，而且完全不可见的时候，就进入了停止状态

    4. 销毁状态：当Activity从栈中移除的时候，此时就进入了销毁状态，系统比较倾向于回收该类的Activity，从而保证内存充足

 * 活动的生存期

    1. onCreate()：主要就是完成界面的初始化，以及事件的绑定，但是此时并没有显示出来

    2. onStart()：此时Activity已经创建好了，并且准备显示的时候，将会调用该方法

    3. onResume()：此时Activity已经显示出来，并且可以和用户进行交互了

    4. onPause()：在系统准备去启动该或恢复另外一个活动的时候调用，可以释放一些消耗CPU的资源，以及保存一些关键数据，速度一定要快，否则会影响新的栈顶activity的使用

    5. onStop()：Activity完全不可见的时候调用，和onPause()的区别在于如果启动的新活动是一个对话框式的活动，那么onPause()会得到执行，而onStop()并不会执行

    6. onDestroy()：在Activity被销毁之前调用，之后Activity变成销毁状态

    7. onRestart()：由停止状态变为运行状态之前调用


  ---
