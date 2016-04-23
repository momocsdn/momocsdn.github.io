---
layout: post
title:  "绝对让你理解Android中的Context"
date:   2015-07-21 23:51:43
categories: android 开发

---
这个问题是StackOverFlow上面一个热门的问题What is Context in Android?
整理这篇文章的目的是Context确实是一个很抽象的东西，我们在项目中随手都会用到它，可是很多人根本不理解它到底是干什么的，这篇文章还会添加Context in Andorid – INSIGHT的翻译，绝对让读者理解Context的意义。

老规矩，作者提出的问题：
在Android中，Context到底是个什么鬼东西，它到底是干嘛使得，我读了很多篇文档，然而并不能清除的理解它的含义。

答案：
简单来说，就像它的名字(上下文)一样，他是项目当前的状态的一个标识，他可以让新创建出来，新加入进来的对象或组件知道当前项目的进度，处于一个什么状态，这样我们就可以容易理解上下文的意思了把，上文就是自己知道了之前项目已经处于一个什么样的状态，下文就是告诉后来的对象或者组件当前项目处于一个什么状态。
你可以通过getApplication()、getContext()、getBaseContext()或者this(在当前的Activity中时)来获取context上下文。
通常使用Context的地方：
创建一个新的对象时：

## 1.创建新的View、adapter、listener

> TextView tv = new TextView(getContext());
> ListAdapter adapter = new SimpleCursorAdapter(getApplicationContext(), ...);

## 2.获取资源文件：例如 LAYOUT_INFLATER_SERVICE, SharedPreferences:

> context.getSystemService(LAYOUT_INFLATER_SERVICE)   
> getApplicationContext().getSharedPreferences(*name*, *mode*);

## 3.隐式访问组件

> getApplicationContext().getContentResolver().query(uri, ...);

如果还不明白不用担心，现在我们开始翻译Context in Andorid – INSIGHT

Context大概是Android项目中最为常用的一个元素了
每个文档中都会有如下一句话：

“An android context is an interface to global information about an application environment”
意思是Android的context是一个沟通全局信息和程序环境的一个接口
当我找一些介绍context的文章时，我发现少之又少，所以我将我看到的一些文章做一个整理。

这里的接口不是Java中接口interface的意思，这个接口就是连接的意思，连接两个组件。

更准确说它是一个代表了各种环境数据的实体。

android.content.context使resources(资源文件)、数据库、文件系统、activity、系统服务等组件之间有了可以访问的入口。

或者可以这样说：Context持有程序的状态、入口、权限、文件系统、等信息，并且是Activity、Service、Application等组件的父类。

在现实世界中我们可以这样描述Context：它就像一张门禁卡，通过这张门禁卡我们可以进入仓库、可以进入客厅、可以进入厨房获取我们想要的资源，这张门禁卡把几个分离的房间连接到一起了。

一个拥有门禁卡（Context）的人（Activity）可以获得各种系统的资源。

我们有3种方式可以获得context：

### 1. mContext = getApplicationContext();
       这种方式获得的context是全局context，整个项目的生命中期中是唯一的且一直存在的，代表了所有activities的context

### 2. mContext = getContext()
	这种方式获得的context当activity销毁时，context也会跟着销毁了

### 3. mContext = getBaseContext();
	说实话我也没用过这种方式
	何时使用getApplicationContext（）或getContext（）？

通过上面的分析我们知道了两个context一个是跟随activity的生命周期一个是跟随application的生命周期的。

因此，当你想获得一个长生命周期的context时，使用Application Context，例如当我们想要使用一个系统的服务时，这个系统服务的周期要比activity的生命周期长，如果我们使用getContext（）的话，当activity销毁时，系统服务也就不能正常进行了，这时候我们就得使用getApplicationContext（）。


  ---
