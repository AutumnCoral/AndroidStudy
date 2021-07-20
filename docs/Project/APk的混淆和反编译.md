# APk的混淆和反编译

## Apk的混淆

[参考文章]https://blog.csdn.net/weixin_45379305/article/details/104885715

混淆的目的就是为了给发编译Apk的时候增加难度，

## 一：打包正式的APk版本

apk文件就是一个包，打包就是要生成apk文件，有了apk别人才能安装使用。打包分debug版和release包，通常所说的打包指生成release版的apk，release版的apk会比debug版的小，release版的还会进行混淆和用自己的keystore签名，以防止别人反编译后重新打包替换你的应用。 

### APK签名和打包

Android系统要求只有有签名的APk才能进行安装，而我们进行debug调试运行的Apk使用的是默认的签名文件进行签名的，因此我们打包正式版本的时候我们就需要生成签名文件进行签名

**签名有两种方式：**

*第一种：*

第一步：点击Build-Generate Signed Bundle/apk；

<img src="G:/wangdandan/note/media/picture/APk的混淆和反编译.assets/image-20201218104624019.png" alt="image-20201218104624019" style="zoom:67%;" />

第二步：选择APk,点击next

<img src="G:/wangdandan/note/media/picture/APk的混淆和反编译.assets/image-20201218104726880.png" alt="image-20201218104726880" style="zoom: 67%;" />

第三步：由于我已经生成，所有存在这些数据，直接点击create new…

<img src="G:/wangdandan/note/media/picture/APk的混淆和反编译.assets/image-20201218105102203.png" alt="image-20201218105102203" style="zoom:67%;" />

第四步：填写信息

<img src="G:/wangdandan/note/media/picture/APk的混淆和反编译.assets/image-20201218110832618.png" alt="image-20201218110832618" style="zoom:67%;" />

签名已经完成

**注意:**

Generate Signed APK 生成签名的APK
key store path 密钥存储库路径       
Password/Confirm：密钥库的密码
Alias：密钥名称(别名)       
Password/Confirm：密钥密码       
Validity(years)：密钥有效期       
First and Last Name：密钥颁发者姓名       
Organizational Unit：密钥颁发组织（组织单位）
Organization:组织（company）      
City or Locality：城市或地区
state or provice:省       
Country Code(XX)：国家代码（cn）

书写签名也可以在代码中

```
signingConfigs{
        release{
            keyAlias 'key的名字，还记得我是（guitar）吗？'
            keyPassword  '你的密码'
            storeFile file('G:/Android_jks/guitar.jks')
            storePassword '还记得让你俩个密码相同吗，就是以防搞混噢'
        }
    }
```

## 二：APk的混淆

[参考文章]https://blog.csdn.net/uu00soldier/article/details/54598677

混淆官方说明： https://developer.android.com/studio/build/shrink-code.html

混淆的教程：http://blog.csdn.net/u011889786/article/details/50945693

https://blog.csdn.net/jungle_pig/article/details/79725974

此教程里面讲得非常详细，各个参数、用法都有非常详细的解释。

参考资料： 

> 简书：http://www.jianshu.com/p/e19cc5194a31
>
> 微信：https://mp.weixin.qq.com/s/ZqV3xrxjM8pFiidDEP7A2w
>
> 参数说明：http://lib.csdn.net/article/android/48396

代码混淆分三个基本步骤：

> 1、  加入基本混淆项
>
> 2、  针对该工程的混淆项
>
> 3、  针对第三方库的解决

### 参数说明

proguard中一共有三组六个keep关键字，说明如下

| ***\*关键字\****               | ***\*描述\****                                               |
| ------------------------------ | ------------------------------------------------------------ |
| **keep**                       | 保留类和类中的成员，防止它们被混淆或移除。                   |
| **keepnames**                  | 保留类和类中的成员，防止它们被混淆，但当成员没有被引用时会被移除。 |
| **keepclassmembers**           | 只保留类中的成员，防止它们被混淆或移除。                     |
| **keepclassmembernames**       | 只保留类中的成员，防止它们被混淆，但当成员没有被引用时会被移除。 |
| **keepclasseswithmembers**     | 保留类和类中的成员，防止它们被混淆或移除，前提是指名的类中的成员必须存在，如果不存在则还是会混淆。 |
| **keepclasseswithmembernames** | 保留类和类中的成员，防止它们被混淆，但当成员没有被引用时会被移除，前提是指名的类中的成员必须存在，如果不存在则还是会混淆。 |

**Proguard中通配符的说明：**

| ***\*通配符\**** | ***\*描述\****                                               |
| ---------------- | ------------------------------------------------------------ |
| **<field>**      | 匹配类中的所有字段                                           |
| **<method>**     | 匹配类中的所有方法                                           |
| **<init>**       | 匹配类中的所有构造函数                                       |
| *****            | 匹配任意长度字符，但不含包名分隔符(.)。比如说我们的完整类名是com.example.test.MyActivity，使用com.*，或者com.exmaple.*都是无法匹配的，因为*无法匹配包名中的分隔符，正确的匹配方式是com.exmaple.*.*，或者com.exmaple.test.*，这些都是可以的。但如果你不写任何其它内容，只有一个*，那就表示匹配所有的东西。 |
| ***\***          | 匹配任意长度字符，并且包含包名分隔符(.)。比如proguard-android.txt中使用的-dontwarn android.support.**就可以匹配android.support包下的所有内容，包括任意长度的子包。 |
| ***\****         | 匹配任意参数类型。比如void set*(***)就能匹配任意传入的参数类型，*** get*()就能匹配任意返回值的类型。 |
| **…**            | 匹配任意长度的任意类型参数。比如void test(…)就能匹配任意void test(String a)或者是void test(int a, String b)这些方法。 |

### 代码混淆

##### 第一步：混淆配置

```
android{
	buildTypes {
        release {
            minifyEnabled true //true代表打开混淆
            shrinkResources true  //true代表打开资源压缩
            zipAlignEnabled true //zipAlign可以让安装包中的资源按4字节对齐，这样可以减少应用在运行时的内存消耗
             // 前一部分代表系统默认的android程序的混淆文件，该文件已经包含了基本的混淆声明，后一个文件是自己的定义混淆文件
            proguardFiles getDefaultProguardFile('proguard-android.txt'), 'proguard-rules.pro'
         }
     }
}
```

##### 第二步：混淆规则

###### 1，基础指令

```
# 代码混淆压缩比，在0~7之间
-optimizationpasses 5
# 混合时不使用大小写混合，混合后的类名为小写
-dontusemixedcaseclassnames
# 指定不去忽略非公共库的类
-dontskipnonpubliclibraryclasses
# 不做预校验，preverify是proguard的四个步骤之一，Android不需要preverify，去掉这一步能够加快混淆速度。
-dontpreverify
-verbose
# 避免混淆泛型
-keepattributes Signature

# 保留Annotation不混淆
-keepattributes *Annotation*,InnerClasses
#google推荐算法
-optimizations !code/simplification/arithmetic,!code/simplification/cast,!field/*,!class/merging/*
# 避免混淆Annotation、内部类、泛型、匿名类
-keepattributes *Annotation*,InnerClasses,Signature,EnclosingMethod
# 重命名抛出异常时的文件名称
-renamesourcefileattribute SourceFile
# 抛出异常时保留代码行号
-keepattributes SourceFile,LineNumberTable
# 处理support包
-dontnote android.support.**
-dontwarn android.support.**
```

###### 2，默认保留区

```
#四大组件、Fragment、自定义控件不需要添加混淆规则，因为这些默认是不会被混淆的，所以以下可能没有必要添加
########################################## Copy From proguard-android.txt ##########################################

-dontusemixedcaseclassnames
-dontskipnonpubliclibraryclasses
-verbose

# Optimization is turned off by default. Dex does not like code run
# through the ProGuard optimize and preverify steps (and performs some
# of these optimizations on its own).
-dontoptimize
-dontpreverify
# Note that if you want to enable optimization, you cannot just
# include optimization flags in your own project configuration file;
# instead you will need to point to the
# "proguard-android-optimize.txt" file instead of this one bind your
# project.properties file.

-keepattributes *Annotation*
-keep public class com.google.vending.licensing.ILicensingService
-keep public class com.android.vending.licensing.ILicensingService

# For native methods, see http://proguard.sourceforge.net/manual/examples.html#native
-keepclasseswithmembernames class * {
    native <methods>;
}
-keepclasseswithmembers class * { # 保持自定义控件类不被混淆
   public <init>(android.content.Context, android.util.AttributeSet);
}
-keepclasseswithmembers class * { # 保持自定义控件类不被混淆
   public <init>(android.content.Context, android.util.AttributeSet, int);
}

# 保留在Activity中的方法参数是view的方法，
# 这样以来我们在layout中写的onClick就不会被影响
# We want to keep methods in Activity that could be used in the XML attribute onClick
-keepclassmembers class * extends android.app.Activity {
   public void *(android.view.View);
}
# keep setters in Views so that animations can still work.
# see http://proguard.sourceforge.net/manual/examples.html#beans
-keepclassmembers public class * extends android.view.View {
   void set*(***);
   *** get*();
}
# 保留枚举类不被混淆
# For enumeration classes, see http://proguard.sourceforge.net/manual/examples.html#enumerations
-keepclassmembers enum * {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}
# 保留Parcelable序列化类不被混淆
-keepclassmembers class * implements android.os.Parcelable {
  public static final android.os.Parcelable$Creator CREATOR;
}
# 保留Serializable序列化的类不被混淆
-keepclassmembers class * implements java.io.Serializable {
    static final long serialVersionUID;
    private static final java.io.ObjectStreamField[] serialPersistentFields;
    !static !transient <fields>;
    !private <fields>;
    !private <methods>;
    private void writeObject(java.io.ObjectOutputStream);
    private void readObject(java.io.ObjectInputStream);
    java.lang.Object writeReplace();
    java.lang.Object readResolve();
}
# 保留R下面的资源
-keepclassmembers class **.R$* {
    public static <fields>;
}

# The support library contains references to newer platform versions.
# Don't warn about those in case this app is linking against an older
# platform version.  We know about them, and they are safe.
-dontwarn android.support.**

#保留Keep注解的类名和方法
# Understand the @Keep support annotation.
-keep class android.support.annotation.Keep

-keep @android.support.annotation.Keep class * {*;}

-keepclasseswithmembers class * {
    @android.support.annotation.Keep <methods>;
}

-keepclasseswithmembers class * {
    @android.support.annotation.Keep <fields>;
}

-keepclasseswithmembers class * {
    @android.support.annotation.Keep <init>(...);
}

########################################## Global ##########################################

# 禁用代码压缩，即不删除没有引用的代码
#-dontshrink

-keepattributes *Annotation*,InnerClasses,*Annotation*,Signature,EnclosingMethod

 # 保持 native 方法不被混淆
-keepclasseswithmembers class * {
    native <methods>;
}

-dontwarn javax.**
-keep,allowshrinking class javax.** {*;}
-keep,allowshrinking enum javax.** {*;}
-keep,allowshrinking interface javax.** {*;}

-dontwarn java.**
-keep,allowshrinking class java.** {*;}
-keep,allowshrinking enum java.** {*;}
-keep,allowshrinking interface java.** {*;}

#保留support下的所有类及其内部类
-dontwarn android.support.**
-keep,allowshrinking class android.support.** {*;}
-keep,allowshrinking enum android.support.** {*;}
-keep,allowshrinking interface android.support.** {*;}

-dontwarn com.android.**
-keep,allowshrinking class com.android.** {*;}
-keep,allowshrinking enum com.android.** {*;}
-keep,allowshrinking interface com.android.** {*;}

-dontwarn com.google.**
-keep,allowshrinking class com.google.** {*;}
-keep,allowshrinking enum com.google.** {*;}
-keep,allowshrinking interface com.google.** {*;}
```

###### 2，第三方库的混淆

第三方框架一般都有写好的混淆规则搬过来就可以了

一下为几种常见的第三方混淆

```
# butterknife
-dontwarn butterknife.**
-keep class butterknife.** {*;}
-keep enum butterknife.** {*;}
-keep interface butterknife.** {*;}

# json
# removes such information by default, so configure it to keep all of it.
-keepattributes Signature
# Gson specific classes
-keep class sun.misc.Unsafe { *; }
-keep class com.google.gson.stream.** { *; }
# Application classes that will be serialized/deserialized over Gson
-keep class com.google.gson.examples.android.model.** { *; }
-keep class com.google.gson.** { *;}
#这句非常重要，主要是滤掉 com.demo.demo.bean包下的所有.class文件不进行混淆编译,com.demo.demo是你的包名
-keep class com.demo.demo.bean.** {*;}

#okgo, okrx, okrx2, okserver 所有代码均可以混淆,但是由于底层使用的是 okhttp,它不能混淆,所以只需要添加以下混淆代码就可以了
#okhttp
-dontwarn okhttp3.**
-keep class okhttp3.**{*;}
#okio
-dontwarn okio.**
-keep class okio.**{*;}


#Glide
-dontwarn com.bumptech.glide.**
-keep class com.bumptech.glide.**{*;}
-keep public class * implements com.bumptech.glide.module.GlideModule
-keep public class * extends com.bumptech.glide.AppGlideModule
-keep public enum com.bumptech.glide.load.resource.bitmap.ImageHeaderParser$** {
  **[] $VALUES;
  public *;
}


# BaseRecyclerViewAdapterHelper
-keep class com.chad.library.adapter.** {
*;
}
-keep public class * extends com.chad.library.adapter.base.BaseQuickAdapter
-keep public class * extends com.chad.library.adapter.base.BaseViewHolder
-keepclassmembers public class * extends com.chad.library.adapter.base.BaseViewHolder {
           <init>(android.view.View);
}

# 百度地图（jar包换成自己的版本，记得签名要匹配）
-libraryjars libs/BaiduLBS_Android.jar
-keep class com.baidu.** {*;}
-keep class vi.com.** {*;}
-keep class com.sinovoice.** {*;}
-keep class pvi.com.** {*;}
-dontwarn com.baidu.**
-dontwarn vi.com.**
-dontwarn pvi.com.**

#EventBus
-keepattributes *Annotation*
-keepclassmembers class ** {
    @org.greenrobot.eventbus.Subscribe <methods>;
}
-keep enum org.greenrobot.eventbus.ThreadMode { *; }


#8 mqtt 可以混淆

#9 mqtt service 可以混淆

# BRVAH
-keep class com.chad.library.adapter.** { *; }
-keep public class * extends com.chad.library.adapter.base.BaseQuickAdapter
-keep public class * extends com.chad.library.adapter.base.BaseViewHolder
-keepclassmembers public class * extends com.chad.library.adapter.base.BaseViewHolder {
    <init>(android.view.View);
}

# Bugly
-dontwarn com.tencent.bugly.**
-keep class com.tencent.bugly.** {*;}

# Dagger2
-dontwarn com.google.errorprone.annotations.*

# Facebook
-keep class com.facebook.** {*;}
-keep interface com.facebook.** {*;}
-keep enum com.facebook.** {*;}


# FastJson
-dontwarn com.alibaba.fastjson.**
-keep class com.alibaba.fastjson.** { *; }
-keepattributes Signature
-keepattributes *Annotation*


# Fresco
-keep class com.facebook.fresco.** {*;}
-keep interface com.facebook.fresco.** {*;}
-keep enum com.facebook.fresco.** {*;}


# 高德相关依赖
# 集合包:3D地图3.3.2 导航1.8.0 定位2.5.0
-dontwarn com.amap.api.**
-dontwarn com.autonavi.**
-keep class com.amap.api.**{*;}
-keep class com.autonavi.**{*;}
# 地图服务
-dontwarn com.amap.api.services.**
-keep class com.map.api.services.** {*;}
# 3D地图
-dontwarn com.amap.api.mapcore.**
-dontwarn com.amap.api.maps.**
-dontwarn com.autonavi.amap.mapcore.**
-keep class com.amap.api.mapcore.**{*;}
-keep class com.amap.api.maps.**{*;}
-keep class com.autonavi.amap.mapcore.**{*;}
# 定位
-dontwarn com.amap.api.location.**
-dontwarn com.aps.**
-keep class com.amap.api.location.**{*;}
-keep class com.aps.**{*;}
# 导航
-dontwarn com.amap.api.navi.**
-dontwarn com.autonavi.**
-keep class com.amap.api.navi.** {*;}
-keep class com.autonavi.** {*;}

### greenDAO 3
-keepclassmembers class * extends org.greenrobot.greendao.AbstractDao {
public static java.lang.String TABLENAME;
        94  }
-keep class **$Properties
 
# If you do not use SQLCipher:
-dontwarn org.greenrobot.greendao.database.**
# If you do not use RxJava:
-dontwarn rx.**


# Gson
-keepattributes Signature
-keepattributes *Annotation*
-keep class sun.misc.Unsafe { *; }
-keep class com.google.gson.stream.** { *; }
# 使用Gson时需要配置Gson的解析对象及变量都不混淆。不然Gson会找不到变量。
# 将下面替换成自己的实体类
#-keep class com.example.bean.** { *; }


# Jackson
-dontwarn org.codehaus.jackson.**
-dontwarn com.fasterxml.jackson.databind.**
-keep class org.codehaus.jackson.** { *;}
-keep class com.fasterxml.jackson.** { *; }


# 极光推送
-dontoptimize
-dontpreverify
-dontwarn cn.jpush.**
-keep class cn.jpush.** { *; }


# OkHttp
-dontwarn okio.**
-dontwarn okhttp3.**
-dontwarn javax.annotation.Nullable
-dontwarn javax.annotation.ParametersAreNonnullByDefault


# Okio
-dontwarn com.squareup.**  
-dontwarn okio.**  
-keep public class org.codehaus.* { *; }  
-keep public class java.nio.* { *; }


# OrmLite
-keepattributes *DatabaseField* 
-keepattributes *DatabaseTable* 
-keepattributes *SerializedName*  
-keep class com.j256.**
-keepclassmembers class com.j256.** { *; }
-keep enum com.j256.**
-keepclassmembers enum com.j256.** { *; }
-keep interface com.j256.**
-keepclassmembers interface com.j256.** { *; }


# Realm
-keep class io.realm.annotations.RealmModule
-keep @io.realm.annotations.RealmModule class *
-keep class io.realm.internal.Keep
-keep @io.realm.internal.Keep class * { *; }
-dontwarn javax.**
-dontwarn io.realm.**


# Retrofit
-keep class retrofit2.** { *; }
-dontwarn retrofit2.**
-keepattributes Signature
-keepattributes Exceptions
-dontwarn okio.**
-dontwarn javax.annotation.**


# Retrolambda
-dontwarn java.lang.invoke.*


# RxJava RxAndroid
-dontwarn sun.misc.**
-keepclassmembers class rx.internal.util.unsafe.*ArrayQueue*Field* {
    long producerIndex;
    long consumerIndex;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueProducerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode producerNode;
}
-keepclassmembers class rx.internal.util.unsafe.BaseLinkedQueueConsumerNodeRef {
    rx.internal.util.atomic.LinkedQueueNode consumerNode;
}
-dontnote rx.internal.util.PlatformDependent


# Universal-Image-Loader-v1.9.5
-libraryjars libs/universal-image-loader-1.9.5-SNAPSHOT-with-sources.jar
-dontwarn com.nostra13.universalimageloader.**
-keep class com.nostra13.universalimageloader.** { *; }


# 微信支付
-dontwarn com.tencent.mm.**
-dontwarn com.tencent.wxop.stat.**
-keep class com.tencent.mm.** {*;}
-keep class com.tencent.wxop.stat.**{*;}


# 信鸽
-keep public class * extends android.app.Service
-keep public class * extends android.content.BroadcastReceiver
-keep class com.tencent.android.tpush.**  {* ;}
-keep class com.tencent.mid.**  {* ;}
-keepattributes *Annotation*


# 新浪微博
-keep class com.sina.weibo.sdk.* { *; }
-keep class android.support.v4.* { *; }
-keep class com.tencent.* { *; }
-keep class com.baidu.* { *; }
-keep class lombok.ast.ecj.* { *; }
-dontwarn android.support.v4.**
-dontwarn com.tencent.**s
-dontwarn com.baidu.**


# XLog
-keep class com.tencent.mars.** { *; }
-keepclassmembers class com.tencent.mars.** { *; }
-dontwarn com.tencent.mars.**


# 讯飞语音
-dontwarn com.iflytek.**
-keep class com.iflytek.** {*;}


# xUtils3.0
-keepattributes Signature,Annotation
-keep public class org.xutils.** {
public protected *;
}
-keep public interface org.xutils.** {
public protected *;
}
-keepclassmembers class * extends org.xutils.** {
public protected *;
}
-keepclassmembers @org.xutils.db.annotation.* class * {;}
-keepclassmembers @org.xutils.http.annotation. class * {*;}
-keepclassmembers class * {
@org.xutils.view.annotation.Event ;
}


# 银联
-dontwarn com.unionpay.**
-keep class com.unionpay.** { *; }


# 友盟统计分析
-keepclassmembers class * { public <init>(org.json.JSONObject); }
-keepclassmembers enum com.umeng.analytics.** {
    public static **[] values();
    public static ** valueOf(java.lang.String);
}


# 友盟自动更新
-keepclassmembers class * { public <init>(org.json.JSONObject); }
-keep public class cn.irains.parking.cloud.pub.R$*{ public static final int *; }
-keep public class * extends com.umeng.**
-keep class com.umeng.** { *; }


# 支付宝钱包
-dontwarn com.alipay.**
-dontwarn HttpUtils.HttpFetcher
-dontwarn com.ta.utdid2.**
-dontwarn com.ut.device.**
-keep class com.alipay.android.app.IAlixPay{*;}
-keep class com.alipay.android.app.IAlixPay$Stub{*;}
-keep class com.alipay.android.app.IRemoteServiceCallback{*;}
-keep class com.alipay.android.app.IRemoteServiceCallback$Stub{*;}
-keep class com.alipay.sdk.app.PayTask{ public *;}
-keep class com.alipay.sdk.app.AuthTask{ public *;}
-keep class com.alipay.mobilesecuritysdk.*
-keep class com.ut.*

#不混淆webview中对javascript接口
-keep public class com.igrs.dlna.activity.webviewActivity.JavaScriptInterface

-keepclassmembers class com.igrs.dlna.activity.webviewActivity.JavaScriptInterface{
void showSource(java.lang.String,java.lang.String);
} 
-keepclassmembers class com.igrs.dlna.activity.webviewActivity$InJavaScriptLocalObj { 
    public void showSource(java.lang.String,java.lang.String);
    public void showTitle(java.lang.String);
}

```

4，针对该工程的混淆项

```
#------------------------------------------1.实体类--------------------------------------------------
#由于实体类要和服务器交互，会将实体属性写成json字段，因此不能覆盖
-keep class com.yl.seismic.exploxxxxon.ui.common.xxxx {*;}
-keep class com.yl.xxxxsmic.exploxxxxion.ui.common.xxxx {*;}
-keep class com.yl.seismic.exploraxxxxion.ui.common.xxxxrmarkMold {*;}
-keep class com.yl.seismic.exploration.ui.common.picture.Picture {*;}
-keep class com.yl.seismic.exploration.ui.home.xxxxMultipleEntity {*;}
-keep class com.yl.seixxxxsmic.exploration.ui.home.xxxxrInfo {*;}
-keep class com.yl.seismic.exploration.ui.homexxxxup {*;}
-keep class com.yl.seismic.exploration.ui.home.conxxxxmber {*;}
-keep class com.yl.seismic.exploration.ui.home.event.EventInfo {*;}
-keep class com.yl.seixxxxsmic.exploration.ui.home.info.xxxxowledge.Knowledge {*;}
-keep class com.yl.seismic.exploration.uixxxx.Notice {*;}
-keep class com.yl.seismic.exploration.ui.home.info.task.TaskInfo {*;}
-keep class com.yl.seismic.xxxx.ui.hoxxxxn.InvestigationInfo {*;}
-keep class com.yl.seixxxxsmic.xxxxloration.ui.home.quicxxxxionInfo {*;}
-keep class com.yl.seismic.exploration.ui.hxxxxrveyInfo {*;}
-keep class com.yl.seismic.exploration.ui.home.xxxxeyMonomer {*;}
-keep class com.xxxxc.exploration.ui.start.xxxx.LaunchVersion {*;}
-keep class com.yl.seismic.expxxxxation.ui.start.loxxxxn.User {*;}
-keep class com.yl.seismic.exxxxtion.ui.start.rxxxxrtment.Department {*;}
```

## 三，不能混淆的情况总结

- Java的反射，为什么不能混淆呢？因为代码混淆，类名、方法名、属性名都改变了，而反射它还是按照原来的名字去反射，结果程序崩溃

- 注解用了反射，所以不能混淆。

- JNI方法不可混淆，因为这个方法需要和native方法保持一致，否则找不到方法

- AndroidMainfest中的类不混淆，所以四大组件和Application的子类和Framework层下所有的类默认不会进行混淆

- 自定义view也是带了包名写在xml布局中，不能混淆

- 与服务端交互时，使用GSON、fastjson等框架解析服务端数据时，所写的JSON对象类不混淆，否则无法将JSON解析成对应的对象

- Parcelable的子类和Creator静态成员变量不混淆，否则会产生Android.os.BadParcelableException异常

- 使用enum类型时需要注意避免以下两个方法混淆，因为enum类的特殊性，以下两个方法会被反射调用

  ```javascript
  -keepclassmembers enum * {  
     public static **[] values();  
     public static ** valueOf(java.lang.String);  
  }
  1234
  ```

- WebView的JS调用需要保证写的接口方法不混淆，否则找不到方法

- R.java文件不能混淆，混淆之后找不到资源

- 泛型不能混淆，不然会报类型转换错误

四，混淆出现的错误：

###### 1，Android代码混淆时出现EventBus - Subscriber class and its super classes have no public methods with the @sub

解决方案：在混淆配置文件中添加如下代码就可以解决了

```
-keepattributes *Annotation*
-keepclassmembers class ** {
    @org.greenrobot.eventbus.Subscribe <methods>;
}
-keep enum org.greenrobot.eventbus.ThreadMode { *; }
```

###### 2，Android - 百度地图打包混淆代码后地图崩溃的解决方法

```
1. 
   \#-libraryjars libs/baidumapapi_v3_1_0.jar 替换成自己所用版本的jar
2. -keep **class** **com**.**baidu**.** { *; }
3. -keep **class** **vi**.**com**.**gdi**.**bgl**.**android**.**{*;}
```

## 四，APk的反编译

[参考文章]https://blog.csdn.net/p312011150/article/details/81098415

###### 使用工具：   官方最新版本下载地址：

​    [apktool](https://code.google.com/p/android-apktool/downloads/list)（google code）

**[ ](https://code.google.com/p/android-apktool/downloads/list)** [**dex2jar**](http://code.google.com/p/dex2jar/downloads/list)（google code） 

[ ](https://code.google.com/p/android-apktool/downloads/list) [jd-gui](http://code.google.com/p/innlab/downloads/list)（google code）最新版请见[官方](http://java.decompiler.free.fr/?q=jdgui)

###### 工具介绍：

**apktool***

*作用：资源文件获取，可以提取出图片文件和布局文件进行使用查看**

**dex2jar**

   作用：将apk反编译成java源码（classes.dex转化成jar文件）

**jd-gui**

  **** **作用：查看APK中classes.dex转化成出的jar文件，即源码文件**用于将jar文件转换成java代码

###### 步骤：

1. 将得到的APk文件包后缀名重新命名成zip，并解压
2. 在解压的文件中有个名为**classes.dex**的文件并找到
3. 打开命令行，进入到**dex2jar**包所在的路径下，输入***\*d2j-dex2jar classes.dex\****，得到**classes-dex2jar.jar这个文件**
4. 用**jd-gui**打开*classes-dex2jar.jar文件

![image-20201218134019449](G:/wangdandan/AndroidInterview/media/pictures/Android.assets/image-20201218134019449-1617083674031.png)。

