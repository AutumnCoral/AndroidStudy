# 自定义控件

# Android中attrs.xml文件的使用详解

[参考文章]https://blog.csdn.net/u011033906/article/details/54934103

<img src="G:/wangdandan/AndroidInterview/docs/media/picture/Android 的自定义属性.assets/10294405-49437ed26f19f7d5.png" alt="img" style="zoom:67%;" />

## 1. 自定义View基础

【参考文章】https://www.cnblogs.com/aademeng/articles/11032816.html

### 1.1 分类

自定义View的实现方式有以下几种

| 类型              | 定义                                                         |
| ----------------- | ------------------------------------------------------------ |
| 自定义组合控件    | 多个控件组合成为一个新的控件，方便多处复用                   |
| 继承系统View控件  | 继承自TextView等系统控件，在系统控件的基础功能上进行扩展     |
| 继承View          | 不复用系统控件逻辑，继承View进行功能定义                     |
| 继承系统ViewGroup | 继承自LinearLayout等系统控件，在系统控件的基础功能上进行扩展 |
| 继承ViewViewGroup | 不复用系统控件逻辑，继承ViewGroup进行功能定义                |

### 1.2 View绘制流程

View的绘制基本由measure()、layout()、draw()这个三个函数完成

| 函数      | 作用                         | 相关方法                                     |
| --------- | ---------------------------- | -------------------------------------------- |
| measure() | 测量View的宽高               | measure(),setMeasuredDimension(),onMeasure() |
| layout()  | 计算当前View以及子View的位置 | layout(),onLayout(),setFrame()               |
| draw()    | 视图的绘制工作               | draw(),onDraw()                              |

### 1.3 坐标系

在Android坐标系中，以屏幕左上角作为原点，这个原点向右是X轴的正轴，向下是Y轴正轴。如下所示：

<img src="G:/wangdandan/AndroidInterview/docs/media/picture/Android 的自定义属性.assets/10294405-a57f0f703dca0eb4.png" alt="img" style="zoom:67%;" />

除了Android坐标系，还存在View坐标系，View坐标系内部关系如图所示。



<img src="G:/wangdandan/AndroidInterview/docs/media/picture/Android 的自定义属性.assets/10294405-4ca426e6a92db696.png" alt="img" style="zoom:67%;" />

视图坐标系.png

#### View获取自身高度

由上图可算出View的高度：

- width = getRight() - getLeft();
- height = getBottom() - getTop();

View的源码当中提供了getWidth()和getHeight()方法用来获取View的宽度和高度，其内部方法和上文所示是相同的，我们可以直接调用来获取View得宽高。

#### View自身的坐标

通过如下方法可以获取View到其父控件的距离。

- getTop()；获取View到其父布局顶边的距离。
- getLeft()；获取View到其父布局左边的距离。
- getBottom()；获取View到其父布局底边的距离。
- getRight()；获取View到其父布局右边的距离。

### 1.4 构造函数

无论是我们继承系统View还是直接继承View，都需要对构造函数进行重写，构造函数有多个，至少要重写其中一个才行。如我们新建`TestView`，

```
public class TestView extends View {
    /**
     * 在java代码里new的时候会用到
     * @param context
     */
    public TestView(Context context) {
        super(context);
    }

    /**
     * 在xml布局文件中使用时自动调用
     * @param context
     */
    public TestView(Context context, @Nullable AttributeSet attrs) {
        super(context, attrs);
    }

    /**
     * 不会自动调用，如果有默认style时，在第二个构造函数中调用
     * @param context
     * @param attrs
     * @param defStyleAttr
     */
    public TestView(Context context, @Nullable AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
    }


    /**
     * 只有在API版本>21时才会用到
     * 不会自动调用，如果有默认style时，在第二个构造函数中调用
     * @param context
     * @param attrs
     * @param defStyleAttr
     * @param defStyleRes
     */
    @RequiresApi(api = Build.VERSION_CODES.LOLLIPOP)
    public TestView(Context context, @Nullable AttributeSet attrs, int defStyleAttr, int defStyleRes) {
        super(context, attrs, defStyleAttr, defStyleRes);
    }
}
```

### 1.5 自定义属性

Android系统的控件以android开头的都是系统自带的属性。为了方便配置自定义View的属性，我们也可以自定义属性值。
Android自定义属性可分为以下几步:

1. 自定义一个View

2. 编写values/attrs.xml，在其中编写styleable和item等标签元素

3. 在布局文件中View使用自定义的属性（注意namespace）

4. 在View的构造方法中通过TypedArray获取

   ### 实例说明

   - 自定义属性的声明文件

     ```
     <declare-styleable name="ColorTrackView">
         <attr name="text" />
         <attr name="text_size" />
         <attr name="text_origin_color" />
         <attr name="text_change_color" />
         <attr name="progress" />
         <attr name="direction" />
     </declare-styleable>
     ```

   - 自定义view

     ```
     public class ColorTrackView extends View {
     
         public ColorTrackView(Context context, AttributeSet attrs) {
             super(context, attrs);
     
             // 创建一个画笔;
             mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
             //加载自定义属性集合CustomView
             TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.ColorTrackView);
             //解析CustomView中自定义的属性
             mText = ta.getString(R.styleable.ColorTrackView_text);
             mTextSize = ta.getDimensionPixelSize(R.styleable.ColorTrackView_text_size, mTextSize);
             mTextOriginColor = ta.getColor(R.styleable.ColorTrackView_text_origin_color, mTextOriginColor);
             mTextChangeColor = ta.getColor(R.styleable.ColorTrackView_text_change_color, mTextChangeColor);
             mProgress = ta.getFloat(R.styleable.ColorTrackView_progress, 0);
             mDirection = ta.getInt(R.styleable.ColorTrackView_direction, mDirection);
             //释放资源，方便下一次使用
             ta.recycle();
             //初始化画笔的参数
             mPaint.setTextSize(mTextSize);
     
         }
     ```

     - 布局文件中使用

       ​	

       ```
       <com.yl.seismic.exploration.tool.view.ColorTrackView
           android:id="@+id/color_view"
           android:layout_width="wrap_content"
           android:layout_height="wrap_content"
           android:layout_margin="@dimen/dimen_10dp"
           android:gravity="center_vertical"
           app:direction="left"
           app:progress="0.0"
           app:text="灾情快速调查评估"
           app:text_change_color="@color/color_blue"
           app:text_origin_color="@color/color_message"
           app:text_size="@dimen/text_size_24sp" />
       ```

### 属性值的类型format

(1). reference：参考某一资源ID

- 属性定义：

```
<declare-styleable name = "名称">
     <attr name = "background" format = "reference" />
</declare-styleable>
```

- 属性使用：

```
<ImageView android:background = "@drawable/图片ID"/>
```

(2). color：颜色值

- 属性定义：

```
<attr name = "textColor" format = "color" />
```

- 属性使用：

```
<TextView android:textColor = "#00FF00" />
```

(3). boolean：布尔值

- 属性定义：

```
<attr name = "focusable" format = "boolean" />
```

- 属性使用：

```
<Button android:focusable = "true"/>
```

(4). dimension：尺寸值

- 属性定义：

```
<attr name = "layout_width" format = "dimension" />
```

- 属性使用：

```
<Button android:layout_width = "42dip"/>
```

(5). float：浮点值

- 属性定义：

```
<attr name = "fromAlpha" format = "float" />
```

- 属性使用：

```
<alpha android:fromAlpha = "1.0"/>
```

(6). integer：整型值

- 属性定义：

```
<attr name = "framesCount" format="integer" />
```

- 属性使用：

```
<animated-rotate android:framesCount = "12"/>
```

(7). string：字符串

- 属性定义：

```
<attr name = "text" format = "string" />
```

- 属性使用：

```
<TextView android:text = "我是文本"/>
```

(8). fraction：百分数

- 属性定义：

```
<attr name = "pivotX" format = "fraction" />
```

- 属性使用：

```
<rotate android:pivotX = "200%"/>
```

(9). enum：枚举值

- 属性定义：

```
<declare-styleable name="名称">
    <attr name="orientation">
        <enum name="horizontal" value="0" />
        <enum name="vertical" value="1" />
    </attr>
</declare-styleable>
```

- 属性使用：

```
<LinearLayout  
    android:orientation = "vertical">
</LinearLayout>
```

注意：枚举类型的属性在使用的过程中只能同时使用其中一个，不能 android:orientation = “horizontal｜vertical"

(10). flag：位或运算

- 属性定义：

```
<declare-styleable name="名称">
    <attr name="gravity">
            <flag name="top" value="0x01" />
            <flag name="bottom" value="0x02" />
            <flag name="left" value="0x04" />
            <flag name="right" value="0x08" />
            <flag name="center_vertical" value="0x16" />
            ...
    </attr>
</declare-styleable>
```

- 属性使用：

```
<TextView android:gravity="bottom|left"/>
```

注意：位运算类型的属性在使用的过程中可以使用多个值

(11). 混合类型：属性定义时可以指定多种类型值

- 属性定义：

```
<declare-styleable name = "名称">
     <attr name = "background" format = "reference|color" />
</declare-styleable>
```

- 属性使用：

```
<ImageView
android:background = "@drawable/图片ID" />
或者：
<ImageView
android:background = "#00FF00" />
```

## 2，自定义组合控件

自定义组合控件就是将多个控件组合成为一个新的控件，主要解决多次重复使用同一类型的布局。如我们顶部的HeaderView以及dailog等，我们都可以把他们组合成一个新的控件。

### 2.1编写布局文件

比如自定义一个LineMenu

```
<?xml version="1.0" encoding="utf-8"?>
<RelativeLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:id="@+id/rel_lineitem"
    android:layout_width="match_parent"
    android:layout_height="50dp"
    android:clickable="true"
    style="@style/line_menu"
    android:paddingLeft="@dimen/default_screen_frame"
    android:paddingRight="@dimen/default_screen_frame"
    android:orientation="horizontal" >

<LinearLayout
    android:layout_centerVertical="true"
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:layout_toLeftOf="@id/img_forward"
    android:orientation="horizontal">
    <ImageView
        android:id="@+id/img_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content" />

    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:minWidth="@dimen/min_hight_50"
        android:layout_marginLeft="15dp"
        android:orientation="horizontal">
    <TextView
        android:id="@+id/txt_title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        style="@style/font_text_primary"
        android:text="昵称"
        android:includeFontPadding="false" />
    <TextView
        android:id="@+id/txt_redTips"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:textColor="@color/read_dot_bg"
        android:text="*" />
    </LinearLayout>

    <TextView
        android:id="@+id/txt_content"
        style="@style/font_text_primary"
        android:layout_width="wrap_content"
        android:layout_height="match_parent"
        android:layout_marginLeft="15dp"
        android:ellipsize="end"
        android:hint="@string/hint_no_choice"
        android:includeFontPadding="false"
        android:lines="1" />

</LinearLayout>

    <ImageView
        android:id="@+id/img_forward"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:layout_centerVertical="true"
        android:layout_alignParentRight="true"
        android:src="@drawable/forward32" />

    <TextView
        android:id="@+id/txt_baseline"
        android:layout_alignParentBottom="true"
        android:layout_width="match_parent"
        android:layout_height="1dp"
        android:background="@color/text_color_light_gray4" />

</RelativeLayout>
```

### **2.2 实现构造方法**

```
//因为我们的布局采用RelativeLayout，所以这里继承RelativeLayout。
//关于各个构造方法的介绍可以参考前面的内容
```

```
public LineItem(Context context) {
    super(context);
}

//如果需要从资源文件中加载自定义的属性，则必须重写Constructor(Context context, AttributeSet attrs)此构造方法，属性是定义在attrs.xml中的；
public LineItem(Context context, AttributeSet attrs) {
    super(context, attrs);

    LayoutInflater.from(context).inflate(R.layout.view_lineitem,this,true);
    parseAttribute(context,attrs);
}

public LineItem(Context context, AttributeSet attrs, int defStyleAttr) {
    super(context, attrs, defStyleAttr);
}
  void parseAttribute(Context context ,AttributeSet attrs){
        TypedArray typedArray = context.obtainStyledAttributes(attrs,R.styleable.LineItem);
        this.titleImg = typedArray.getResourceId(R.styleable.LineItem_titleImg,0);
        this.titleText = typedArray.getResourceId(R.styleable.LineItem_titleTxt,0);
        this.valueText = typedArray.getResourceId(R.styleable.LineItem_contentText,0);
        this.hintText = typedArray.getResourceId(R.styleable.LineItem_hintText,0);
        this.showBaseLine = typedArray.getBoolean(R.styleable.LineItem_showBottomLine,true);
        this.showRedTips  = typedArray.getBoolean(R.styleable.LineItem_showRedTips,true);
        this.showValueText = typedArray.getBoolean(R.styleable.LineItem_showContentText,true);
        this.showTitleImg = typedArray.getBoolean(R.styleable.LineItem_showTitleImage,true);
        typedArray.recycle();

    }
```

### **2.3. 初始化UI**

```
void initView(){

    rel_lineitem = findViewById(R.id.rel_lineitem);
    img_title = findViewById(R.id.img_title);
    img_forward = findViewById(R.id.img_forward);
    txt_title = findViewById(R.id.txt_title);
    txt_redTips = findViewById(R.id.txt_redTips);
    txt_content = findViewById(R.id.txt_content);
    txt_baseline = findViewById(R.id.txt_baseline);

    if(titleImg != 0)
        img_title.setImageResource(titleImg);
    if(titleText != 0)
        txt_title.setText(titleText);
    int tipv = showRedTips ? View.VISIBLE : View.INVISIBLE;
    txt_redTips.setVisibility(tipv);

    int baselinev = showBaseLine ? View.VISIBLE : View.INVISIBLE;
    txt_baseline.setVisibility(baselinev);
    int valuev = showValueText ? View.VISIBLE : View.INVISIBLE;
    txt_content.setVisibility(valuev);

    if (hintText !=0)
        txt_content.setHint(hintText);
    int titleimgv = showTitleImg ? View.VISIBLE : View.GONE;
    img_title.setVisibility(titleimgv);	
    rel_lineitem.setOnClickListener(view->{
        if(onLineItemClickListener != null){
            onLineItemClickListener.onItemClick(this);
        }
    });

}
```

### *2.*4. 提供对外的方法**

```
OnLineItemClickListener onLineItemClickListener;

rel_lineitem.setOnClickListener(view->{
        if(onLineItemClickListener != null){
            onLineItemClickListener.onItemClick(this);
        }
    });
//提供一个借口
public interface OnLineItemClickListener{
        void onItemClick(LineItem lineMenu);
    }
}
```

### Android TypedArray详解

[参考文章1]https://blog.csdn.net/abcdef314159/article/details/51692952?utm_medium=distribute.pc_relevant.none-task-blog-OPENSEARCH-1.control&depth_1-utm_source=distribute.pc_relevant.none-task-blog-OPENSEARCH-1.control

[参考文章2]https://blog.csdn.net/qq_36523667/article/details/79289821

