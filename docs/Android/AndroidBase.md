### Android中fitsSystemWindows总结

**属性说明**：fitsSystemWindows让添加了该属性的View可以根据窗口来调整View的布局位置，就是要考虑系统的窗口的位置。

**使用条件**：第一：只有当系统设置为了透明状态（透明状态栏和透明导航栏）或者去掉系统自带的标题栏的是时候才管用。

                      第二：android版本必须是在 Android4.4及以上的系统，因为4.4以下的系统StatusBar没有透明状态。

android:fitsSystemWindows="true"：根据窗口自动调整View布局位置

#### Android隐藏和设置透明窗口的几种方式

最近这边项目中是将标题栏隐藏掉，在滑动布局中加入了该属性，

**方式一（代码实现）**（这里介绍去掉标题栏）

当我们需要对某一个Activity或者Fragment需要隐藏掉系统自带的窗口的时候，我们可以在代码中实现

**在需要去除的类，注意：onCreate（）方法中，setContentView（R.layout.main）之前加入：**

**去掉标题栏**：requestWindowFeature(Window.FEATURE_NO_TITLE)

**去掉状态栏**：getWindow().setFlags(WindowManager.LayoutParams.FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN);

注意：有时候用**requestWindowFeature(Window.FEATURE_NO_TITLE)**去掉标题栏的时候没有反应

解决方式有两种：

一：创建的activity默认继承了AppComxxxxxActivity,把这个改成Activity

二：在onCreate中加入这句：

```
if (getSupportActionBar() != null){
    getSupportActionBar().hide();
}
```

**方式二：AndroidManifest**

在AndroidManifest中设置属性

```
android:theme="@style/AppTheme"
```

在style.xml定义：

```xml
<style name="Theme.AppCompat.NoActionBar">  
         <item name="windowActionBar">false</item>  
         //隐藏标题栏
         <item name="windowNoTitle">true</item>  
         //隐藏状态栏
         <item name="android:windowFullscreen">true</item>
          //透明状态栏
         <item name="android:windowTranslucentStatus">true</item>
</style>  
```

# MarkDark学习

## 标题：

方式一：

一级：(#+空格+标题)

二级：(##+空格+标题)

三级：(###+空格+标题)

方式二：选中文字按 ctrl+数字（1，2，3 。。。）

## 字体：

加粗：**ctrl+b**

斜体：*Ctrl+i*

下滑线：<u>Ctrl+U</u>

清除格式:ctrl+/

左边视图：Ctrl+Shift+1,2,3:

## 引用

>选择java

直接在需要引用的句子前面加上一个>大于号

## 分割线

三个以上的---或者***（注意要回车）

---

***

## 图片

插入图片：**Ctrl+shift+U**   或者  **!+[]()**    ![截图]()图片路径可以是本地也可以是网络的，将网络图片地址复制过来就可以

## 超链接

[TextView](https://www.jianshu.com/p/14b86738ed99)

[]()

## 列表

有序：数字. +空格回车

1. 
2. 

无序：- +空格

- 
- 

## 表格

右键插入

## 代码

```
三个点 +语言
```