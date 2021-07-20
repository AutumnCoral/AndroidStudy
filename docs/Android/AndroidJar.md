# 常见的流行框架



## 第1课 缓存

| 名称                                                        | 描述                      |
| ----------------------------------------------------------- | ------------------------- |
| [DiskLruCache](https://github.com/JakeWharton/DiskLruCache) | Java实现基于LRU的磁盘缓存 |

## 第2课 图片加载

| 名称                                                         | 描述                                 |
| ------------------------------------------------------------ | ------------------------------------ |
| [Android Universal Image Loader](https://github.com/nostra13/Android-Universal-Image-Loader) | 一个强大的加载，缓存，展示图片的库   |
| [Picasso](https://github.com/square/picasso)                 | 一个强大的图片下载与缓存的库         |
| [Fresco](https://github.com/facebook/fresco)                 | 一个用于管理图像和他们使用的内存的库 |
| [Glide](https://github.com/bumptech/glide)                   | 一个图片加载和缓存的库               |

#### 1，Glide

[参考]https://guolin.blog.csdn.net/article/details/53759439

[参考]https://blog.csdn.net/Love667767/article/details/106576888/

[参考]https://blog.csdn.net/yulyu/article/details/55096684

##### 概述

**Glide 框架的优点：**

1. ```
   加载类型多样化：Glide 支持 Gif、WebP 等格式的图片。
   生命周期的绑定：图片请求与页面生命周期绑定，避免内存泄漏。
   使用简单(链式调用)，且提供丰富的 Api 功能 (如: 图片裁剪等功能)。
   高效的缓存策略：
    支持多种缓存策略 (Memory 和 Disk 图片缓存)。
    根据 ImageView 的大小来加载相应大小的图片尺寸。
    内存开销小，默认使用 RGB_565 格式 (3.x 版本)。
    使用 BitmapPool 进行 Bitmap 的复用。
   ```

##### 使用

**导入依赖添加权限：****

```
/***  图片加载框架*/
implementation 'com.github.bumptech.glide:glide:4.11.0'
/***另外，Glide中需要用到网络功能，因此你还得在AndroidManifest.xml中声明一下网络权限才行*/
<uses-permission android:name="android.permission.INTERNET" />
/***加载图片*/
Glide.with(this).load(url).into(imageView);
```

------

**解析：**

- Glide.with(this）：用于创建加载图片的实例，with()方法可以接收Context、Activity或者Fragment类型的参数，因此不管Activity还是Fragment中调用with()方法，都可以直接传this。注意with()方法中传入的实例会决定Glide加载图片的生命周期，如果传入的是Activity或者Fragment的实例，那么当这个Activity或Fragment被销毁的时候，图片加载也会停止。如果传入的是ApplicationContext，那么只有当应用程序被杀掉的时候，图片加载才会停止。

- load(url):指定加载的图片资源,Glide支持加载各种各样的图片资源，包括网络图片、本地图片、应用资源、二进制流、Uri对象等等。

- into：图片加载到那个imageview中。

  **占位图**

  当加载图片的时候，从网络上下载需要一段的时间，这是可以给他设置一张默认临时的图片，当图片加载以后可以在替换掉临时的	

  ```
  Glide.with(this).load(url).placeholder(R.drawable.loading).diskCacheStrategy(DiskCacheStrategy.NONE)).into(imageView);
  ```

  - .diskCacheStrategy(DiskCacheStrategy.NONE):禁用掉Glide的缓存功能,让占位图显示出来
  - .error(R.drawable.error):指定异常占位符

  **指定图片格式**

  glide的强大可以加载gif图片。glide可以自动判断图片的格式，如何需要限制图片的格式，可以调用方法进行限制，比如，限制只能加载静态的图片

  asBitmap()方法，强制加载动态图片 .asGif()；

  **指定图片大小**

  glide可以自动判断图片的大小，不会造成内存的浪费，如果必须给图片设置固定的大小，可以调用这个方法进行 .override(100, 100)大小的限制。

  

##### 源码分析

Glide的源码非常的复杂，如果全部一行一行的去搞懂，这是非常痛苦的一件事，我们可以要有目的性的去阅读，而是应该只分析它的主体实现逻辑，这里我们搞懂Glide.with(this).load(url).into(imageView);这句代码	

glide的三步走：with(),load(),into()

###### **with()**

with()方法是Glide类中的一组静态方法

     @NonNull
      public static RequestManager with(@NonNull Context context) {
        return getRetriever(context).get(context);
      }
      @NonNull
      public static RequestManager with(@NonNull Activity activity) {
        return getRetriever(activity).get(activity);
      }
      @NonNull
      public static RequestManager with(@NonNull FragmentActivity activity) {
        return getRetriever(activity).get(activity);
      }
      @NonNull
      public static RequestManager with(@NonNull Fragment fragment) {
        return getRetriever(fragment.getContext()).get(fragment);
      }
      @SuppressWarnings("deprecation")
      @Deprecated
      @NonNull
      public static RequestManager with(@NonNull android.app.Fragment fragment) {
        return getRetriever(fragment.getActivity()).get(fragment);
      }
      @NonNull
      public static RequestManager with(@NonNull View view) {
        return getRetriever(view.getContext()).get(view);
      }

**解析**：with有六个重载的的方法，他们可以传入Context、Activity、Fragment、View等，方法都是通过获取RequestManagerRetriever得到RequestManager对象。

传入的参数可以分为两类：
**Application类型**，在Glide.with()方法中传入的是一个Application对象，那么这里就会调用带有Context参数的get()方法重载。因为Application对象的生命周期就是应用程序的生命周期，因此它是和应用程序的生命周期时同步的，应用程序关闭时，Glide就会停止加载图片 ；
**非Application类型**，，图片随着这类参数的生命周期而加载，因为这类参数的生命周期时有限的，他们随时都有可能关闭，此时给他们传一个隐藏的Fragment，如果当前的activity关闭了，就不需要继续的去请求网络，此时Glide并不知道，但是fragment可以监听到（fragment绑定在activity中），这样Glide就能够停止继续加载图片。

```
@NonNull
public RequestManager get(@NonNull Context context) {
  if (context == null) {
    throw new IllegalArgumentException("You cannot start a load on a null Context");
  } else if (Util.isOnMainThread() && !(context instanceof Application)) {//instanceof 用来测试一个对象是否为一个类的实例
  //传入的参数是非Application类型
    if (context instanceof FragmentActivity) {
      return get((FragmentActivity) context);
    } else if (context instanceof Activity) {
      return get((Activity) context);
    } else if (context instanceof ContextWrapper
        && ((ContextWrapper) context).getBaseContext().getApplicationContext() != null) {
      return get(((ContextWrapper) context).getBaseContext());
    }
  }
//传入的参数是Application类型
  return getApplicationManager(context);
}
```

**当传入的参数是非Application类型时，调用的get重载方法*

```
/*传入fragment**/
@NonNull
public RequestManager get(@NonNull Fragment fragment) {
  Preconditions.checkNotNull(
      fragment.getContext(),
      "You cannot start a load on a fragment before it is attached or after it is destroyed");
  if (Util.isOnBackgroundThread()) {
    return get(fragment.getContext().getApplicationContext());
  } else {
    FragmentManager fm = fragment.getChildFragmentManager();
    return supportFragmentGet(fragment.getContext(), fm, fragment, fragment.isVisible());
  }
}

/*传入activty**/
  @NonNull
  public RequestManager get(@NonNull Activity activity) {
    if (Util.isOnBackgroundThread()) {//如果我们是在非主线程当中使用的Glide，那么不管你是传入的Activity还是Fragment，都会被强制当成Application来处理
      return get(activity.getApplicationContext());
    } else {
      assertNotDestroyed(activity);
      android.app.FragmentManager fm = activity.getFragmentManager();
      return fragmentGet(activity, fm, /*parentHint=*/ null, isActivityVisible(activity));
    }
  }
/*传入view**/
  @NonNull
  public RequestManager get(@NonNull View view) {
    if (Util.isOnBackgroundThread()) {
      return get(view.getContext().getApplicationContext());
    }

    Preconditions.checkNotNull(view);
    Preconditions.checkNotNull(
        view.getContext(), "Unable to obtain a request manager for a view without a Context");
    Activity activity = findActivity(view.getContext());
    // The view might be somewhere else, like a service.
    if (activity == null) {
      return get(view.getContext().getApplicationContext());
    }

    // Support Fragments.
    // Although the user might have non-support Fragments attached to FragmentActivity, searching
    // for non-support Fragments is so expensive pre O and that should be rare enough that we
    // prefer to just fall back to the Activity directly.
    if (activity instanceof FragmentActivity) {
      Fragment fragment = findSupportFragment(view, (FragmentActivity) activity);
      return fragment != null ? get(fragment) : get((FragmentActivity) activity);
    }

    // Standard Fragments.
    android.app.Fragment fragment = findFragment(view, activity);
    if (fragment == null) {
      return get(activity);
    }
    return get(fragment);
  }
```

**当传入的参数是Application类型时，调用的get重载方法**

```
/**
@NonNull
private RequestManager getApplicationManager(@NonNull Context context) {
  // Either an application context or we're on a background thread.
  if (applicationManager == null) {
    synchronized (this) {
      if (applicationManager == null) {
        // Normally pause/resume is taken care of by the fragment we add to the fragment or
        // activity. However, in this case since the manager attached to the application will not
        // receive lifecycle events, we must force the manager to start resumed using
        // ApplicationLifecycle.

        // TODO(b/27524013): Factor out this Glide.get() call.
        Glide glide = Glide.get(context.getApplicationContext());
        applicationManager =
            factory.build(
                glide,
                new ApplicationLifecycle(),
                new EmptyRequestManagerTreeNode(),
                context.getApplicationContext());
      }
    }
  }

  return applicationManager;
}
```

**添加隐藏的fragment** 

```
@NonNull
private SupportRequestManagerFragment getSupportRequestManagerFragment(
    @NonNull final FragmentManager fm, @Nullable Fragment parentHint, boolean isParentVisible) {
  SupportRequestManagerFragment current =
      (SupportRequestManagerFragment) fm.findFragmentByTag(FRAGMENT_TAG);
  if (current == null) {
    current = pendingSupportRequestManagerFragments.get(fm);
    if (current == null) {
      current = new SupportRequestManagerFragment();
      current.setParentFragmentHint(parentHint);
      if (isParentVisible) {
        current.getGlideLifecycle().onStart();
      }
      pendingSupportRequestManagerFragments.put(fm, current);
      fm.beginTransaction().add(current, FRAGMENT_TAG).commitAllowingStateLoss();
      handler.obtainMessage(ID_REMOVE_SUPPORT_FRAGMENT_MANAGER, fm).sendToTarget();
    }
  }
  return current;
}
```

注意： 不管添加啥，都会给当前的Activity添加一个隐藏的fragment，因为Glide需要知道加载的生命周期，因为fragment和activity的生命周期是同步的，如果Activity被销毁了，Fragment是可以监听到的，这样Glide就可以捕获这个事件并停止图片加载了。

​           with():就是为了得到一个RequestManager对象而已，然后Glide会根据我们传入with()方法的参数来确定图片加载的生命周期，并没有什么特别复杂的逻辑。

###### load()

##### 常见面试题

1，Glide 是如何加载 GIF 动图的？

参考：https://www.jianshu.com/p/c1e4682c1d46

2，我看你写到Glide，为什么用Glide，而不选择其它图片加载框架？

首先，当下流行的图片加载框架有那么几个，可以拿 Glide 跟[Fresco](https://links.jianshu.com/go?to=https%3A%2F%2Fgithub.com%2Ffacebook%2Ffresco)对比，例如这些：

**Glide：**

- 多种图片格式的缓存，适用于更多的内容表现形式（如Gif、WebP、缩略图、Video）
- 生命周期集成（根据Activity或者Fragment的生命周期管理图片加载请求）
- 高效处理Bitmap（bitmap的复用和主动回收，减少系统回收压力）
- 高效的缓存策略，灵活（Picasso只会缓存原始尺寸的图片，Glide缓存的是多种规格），加载速度快且内存开销小（默认Bitmap格式的不同，使得内存开销是Picasso的一半）

**Fresco：**

- 最大的优势在于5.0以下(最低2.3)的bitmap加载。在5.0以下系统，Fresco将图片放到一个特别的内存区域(Ashmem区)
- 大大减少OOM（在更底层的Native层对OOM进行处理，图片将不再占用App的内存）
- 适用于需要高性能加载大量图片的场景

对于一般App来说，Glide完全够用，而对于图片需求比较大的App，为了防止加载大量图片导致OOM，Fresco 会更合适一些。并不是说用Glide会导致OOM，Glide默认用的内存缓存是LruCache，内存不会一直往上涨。

3，有看过它的源码吗？跟其它图片框架相比有哪些优势？

4，假如现在不让你用开源库，需要你自己写一个图片加载框架，你会考虑哪些方面的问题，说说大概的思路

- 异步加载：线程池
- 切换线程：Handler，没有争议吧
- 缓存：LruCache、DiskLruCache
- 防止OOM：软引用、LruCache、图片压缩、Bitmap像素存储位置
- 内存泄露：注意ImageView的正确引用，生命周期管理
- 列表滑动加载的问题：加载错乱、队满任务过多问题

## 第3课 Glide图片加载库

| 名称                                                         | 描述                              |
| ------------------------------------------------------------ | --------------------------------- |
| [Picasso-transformations](https://github.com/wasabeef/picasso-transformations) | 一个为Picasso提供多种图片变换的库 |
| [Glide-transformations](https://github.com/wasabeef/glide-transformations) | 一个为Glide提供多种图片变换的库   |
| [Android-gpuimage](https://github.com/CyberAgent/android-gpuimage) | 基于OpenGL的Android过滤器         |

## 第4课 网络请求

------

| 名称                                                         | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [Android Async HTTP](https://github.com/loopj/android-async-http) | Android异步HTTP库                                            |
| [AndroidAsync](https://github.com/koush/AndroidAsync)        | 异步Socket，HTTP(客户端+服务器)，WebSocket，和socket.io库。基于NIO而不是线程。 |
| [OkHttp](https://github.com/square/okhttp)                   | 一个Http与Http/2的客户端                                     |
| [Retrofit](https://github.com/square/retrofit)               | 类型安全的Http客户端                                         |
| [Volley](https://android.googlesource.com/platform/frameworks/volley) | Google推出的Android异步网络请求框架和图片加载框架            |

#### okhttp

[参考]https://www.jianshu.com/p/da4a806e599b

1，简介

####  Retrofit2

[参考]https://www.jianshu.com/p/f2644cc784f3

#### rxjava

## 第5课  网络解析

------

| 名称                                                      | 描述                                                         |
| --------------------------------------------------------- | ------------------------------------------------------------ |
| [Gson](https://github.com/google/gson)                    | 一个Java序列化/反序列化库，可以将JSON和java对象互相转换      |
| [Jackson](https://github.com/codehaus/jackson)            | Jackson可以轻松地将Java对象转换成json对象和xml文档，同样也可以将json、xml转换成Java对象 |
| [Fastjson](https://github.com/alibaba/fastjson)           | Java上一个快速的JSON解析器/生成器                            |
| [HtmlPaser](https://sourceforge.net/projects/htmlparser/) | 一种用来解析单个独立html或嵌套html的方式                     |
| [Jsoup](https://github.com/jhy/jsoup)                     | 一个以最好的DOM，CSS和jQuery解析html的库                     |

## 第6课   数据库

------

| 名称                                                         | 描述                                            |
| ------------------------------------------------------------ | ----------------------------------------------- |
| [OrmLite](https://sourceforge.net/projects/ormlite/files/releases/com/j256/ormlite/) | JDBC和Android的轻量级ORM java包                 |
| [Sugar](https://github.com/satyan/sugar)                     | 用超级简单的方法处理Android数据库               |
| [GreenDAO](https://github.com/greenrobot/greenDAO)           | 一种轻快地将对象映射到SQLite数据库的ORM解决方案 |
| [ActiveAndroid](https://github.com/pardom/ActiveAndroid)     | 以活动记录方式为Android SQLite提供持久化        |
| [SQLBrite](https://github.com/square/sqlbrite)               | SQLiteOpenHelper 和ContentResolver的轻量级包装  |
| [Realm](https://github.com/jhy/jsoup)                        | 移动数据库：一个SQLite和ORM的替换品             |

## 第7课   依赖注入

------

| 名称                                                         | 描述                                      |
| ------------------------------------------------------------ | ----------------------------------------- |
| [ButterKnife](https://github.com/JakeWharton/butterknife)    | 将Android视图和回调方法绑定到字段和方法上 |
| [Dagger2](https://github.com/google/dagger)                  | 一个Android和java快速依赖注射器。         |
| [AndroidAnotations](https://github.com/excilys/androidannotations) | 快速安卓开发。易于维护                    |
| [RoboGuice](https://github.com/roboguice/roboguice)          | Android平台的Google Guice                 |

## 第8课 极光推送

## 第9课 WebView浏览器组件

## 第10课 依赖注入

## 第11课 图表

------

| 名称                                                         | 描述                             |
| ------------------------------------------------------------ | -------------------------------- |
| [WilliamChart](https://github.com/diogobernardino/WilliamChart) | 创建图表的Android库              |
| [HelloCharts](https://github.com/lecho/hellocharts-android)  | 兼容到API8的Android图表库        |
| [MPAndroidChart](https://github.com/PhilJay/MPAndroidChart)  | 一个强大的Android图表视图/图形库 |

##  第12课 后台处理

------

| 名称                                                         | 描述                                     |
| ------------------------------------------------------------ | ---------------------------------------- |
| [Tape](https://github.com/square/tape)                       | 一个轻快的，事务性的，基于文件的FIFO的库 |
| [Android Priority Job Queue](https://github.com/yigit/android-priority-jobqueue) | 一个专门为Android轻松调度任务的工作队列  |

##  第13课  事件总线

------

| 名称                                               | 描述                                                     |
| -------------------------------------------------- | -------------------------------------------------------- |
| [EventBus](https://github.com/greenrobot/EventBus) | 安卓优化的事件总线，简化了活动、片段、线程、服务等的通信 |
| [Otto](https://github.com/square/otto)             | 一个基于Guava的增强的事件总线                            |

## 第14课  响应式编程

------

| 名称                                                    | 描述                                                       |
| ------------------------------------------------------- | ---------------------------------------------------------- |
| [RxJava](https://github.com/ReactiveX/RxJava)           | JVM上的响应式扩展                                          |
| [RxJavaJoins](https://github.com/ReactiveX/RxJavaJoins) | 为RxJava提供Joins操作                                      |
| [RxAndroid](https://github.com/ReactiveX/RxAndroid)     | Android上的响应式扩展，在RxJava基础上添加了Android线程调度 |
| [RxBinding](https://github.com/JakeWharton/RxBinding)   | 提供用RxJava绑定Android UI的API                            |
| [Agera](https://github.com/google/agera)                | Android上的响应式编程                                      |

## 第15课 Log框架

------

| 名称                                            | 描述                                   |
| ----------------------------------------------- | -------------------------------------- |
| [Logger](https://github.com/orhanobut/logger)   | 简单，漂亮，强大的Android日志工具      |
| [Hugo](https://github.com/JakeWharton/hugo)     | 在调试版本上注解的触发方法进行日志记录 |
| [Timber](https://github.com/JakeWharton/timber) | 一个小的，可扩展的日志工具             |

## 第16课  测试框架

------

| 名称                                                     | 描述                          |
| -------------------------------------------------------- | ----------------------------- |
| [Mockito](https://github.com/mockito/mockito)            | Java编写的Mocking单元测试框架 |
| [Robotium](https://github.com/RobotiumTech/robotium)     | Android UI 测试               |
| [Robolectric](https://github.com/xtremelabs/robolectric) | Android单元测试框架           |

**Android自带很多测试工具：JUnit，Monkeyrunner，UiAutomator，Espresso等**

## 第17课 调试框架

------

| 名称                                         | 描述                                                        |
| -------------------------------------------- | ----------------------------------------------------------- |
| [Stetho](https://github.com/facebook/stetho) | 调试Android应用的桥梁，使得可以利用Chrome开发者工具进行调试 |

## 第18课  性能优化

------

| 名称                                               | 描述                    |
| -------------------------------------------------- | ----------------------- |
| [LeakCanary](https://github.com/square/leakcanary) | 内存泄漏检测工具        |
| [ACRA](https://github.com/ACRA/acra)               | Android应用程序崩溃报告 |

## 第18课 日志监控

| 名称      | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| 腾讯bugly | Bugly 能帮助移动互联网开发者更及时地发现掌控异常，更全面的了解定位异常，更高效的修复解决异常 |

