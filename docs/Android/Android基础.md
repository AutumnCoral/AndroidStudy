# Android基础

## 核心知识点

### 	四大组件

#### 		Activity

Activity是一种可以包含用户界面的组件，主要用户与用户进行交互		

#### 		Service



#### 		BroadcastReceiver



#### 		ContentProvider



### 	布局和控件

#### 		布局

##### 			RelativeLayout

##### 			FrameLayout

##### 	LinearLayout

##### ConstraintLayout

[参考学习](https://www.jianshu.com/p/17ec9bd6ca8a)

1，概述：

**约束布局ConstraintLayout** 是一个ViewGroup，可以在Api9以上的Android系统使用它，它的出现主要是为了解决布局嵌	套过多的问题，以灵活的方式定位和调整小部件。从 **Android Studio 2.3** 起，官方的模板默认使用 **ConstraintLayout**。

[ConstraintLayout 官方文档](https://links.jianshu.com/go?to=https%3A%2F%2Fdeveloper.android.google.cn%2Freference%2Fandroid%2Fsupport%2Fconstraint%2FConstraintLayout)

2，使用原因

ConstraintLayout也是为了解决布局嵌套层级过多而导致界面卡顿和性能与体验降低的问题

3，特点

-  扁平式布局，无须嵌套，一个层级就可以绘制复杂布局；
- 高渲染性能；
- · 集合了线性布局、相对布局、百分比布局的特点和大部分功能于一身；
- · 支持在可视化环境下拖曳绘制约束布局

4，使用

 **使用方式**：新建的项目默认使用，也可以导入依赖

**相对定位**

常用属性

- layout_constraintLeft_toLeftOf
   layout_constraintLeft_toRightOf
   layout_constraintRight_toLeftOf
   layout_constraintRight_toRightOf
   layout_constraintTop_toTopOf
   layout_constraintTop_toBottomOf
   layout_constraintBottom_toTopOf
   layout_constraintBottom_toBottomOf
   layout_constraintBaseline_toBaselineOf
   layout_constraintStart_toEndOf
   layout_constraintStart_toStartOf
   layout_constraintEnd_toStartOf
   layout_constraintEnd_toEndOf

边距

##### 			Button

**Android原生Button去掉阴影**

style="?android:attr/borderlessButtonStyle"



### 	自定义View/ViewGroup

​		onMeasure
​		onLayout
​		onDraw
​		onTouchEvent
​		dispatchTouchEvent
​		自定义属性

### 	动画和手势

​		View动画
​		属性动画
​		layoutAnimation视图动画
​		手势检测(GestureDetector)
​		缩放手势检测(ScaleGestureDecetor)

### 	网络

​		请求网络
​		解析数据

### 	 图片加载

​		本地图片
​		网络图片
​		压缩图片
​		多图列表
​		DiskLruCache

### 	Handler

​		Looper
​		Message
​		MessageQueue
​		内存泄漏
​		ThreadLocal

### 	Android各版本新特性

​		Android5.0
​		Android6.0
​		Android7.0
​		Android8.0(O)
​		Android9.0(P)
​		Android10.0(Q)
​		Android11.0(R)

### 	数据库

​		adb常用命令
​		文件和数据库
​		异步线程池
​		Resources

### 性能优化

​	快-流畅的体验
​		布局优化
​		绘制优化
​		内存优化
​		启动优化
​		其他
​	稳-稳定
​		避免内存泄露
​		避免崩溃
​	省-省电/流量
​		使用JobScheduler调度任务
​		使用懒惰法则
​	 小-安装包小
​		apk构成
​		包体优化

代码管理
	git
	代码规范

### 开源库使用

​	Retrofit/OKhttp
​	RxJava
​	Glide
​	注解框架
​	Jetpack

