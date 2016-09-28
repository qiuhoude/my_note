
# Android绘图与动画笔记

## 绘图机制和处理


### 屏幕尺寸信息

|  密度  |     ldpi    |    mdpi   |     hdpi    |   xhdpi   |   xxhdpi  |
|--------|-------------|-----------|-------------|-----------|-----------|
| 密度值 | 120         | 160       | 240         | 320       | 480       |
| 分辨率 | 240x320     | 320x480   | 480x800     | 720x1280  | 1080x1920 |
| 公式   | 1dp = 3/4px | 1dp = 1px | 1dp = 1.5px | 1dp = 2px | 1dp = 3px |
 

当mdpi 即密度值为160是作为标准, 1dp = 1px  
公式: ldpi : mdpi: hdpi : xhdpi : xxhdpi = 3 : 4 : 6 : 8 : 12  		  
  
单位换算代码 `TypedValue`帮助类:

```java
//dp2px
TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP,
                dp,
                getResources().getDisplayMetrics());
```

### Android xml绘图
[小刚的xml绘图](http://keeganlee.me/post/android/20150830)
##### Shape

```xml
<shape
    xmlns:android="http://schemas.android.com/apk/res/android"
    //默认为rectangle
    android:shape=["rectangle"|"oval"|"line"|"ring"]>
    <corners //只有当shape="rectangle"时使用
        android:radius="integer"  //圆角半径，会被后面的单个半径属性覆盖
        android:topLeftRadius="integer"
        android:topRightRadius="integer"
        android:bottomLegtRadius="integer"
        android:bottomRightRadius="integer"/>
    <gradient   //渐变
        android:angle="integer" //渐变的角度，线性渐变时才有效，必须是45的倍数，0表示从左到右，90表示从下到上
        android:centerX="integer"//渐变中心的相对X坐标，放射渐变时才有效，在0.0到1.0之间，默认为0.5，表示在正中间
        android:centerY="integer" 
        android:startColor="color"//渐变开始的颜色
        android:centerColor="color"//渐变中间的颜色
        android:endColor="color"//渐变结束的颜色
        android:gradientRadius="integer" //渐变的半径，只有渐变类型为radial时才使用
         //linear 线性渐变，默认的渐变类型 
         //radial 放射渐变，设置该项时，android:gradientRadius也必须设置
         //sweep 扫描性渐变
        android:type=["linear"|"radial"|"sweep"] //渐变的类型 
        android:useCenter=["true"|"false"]/> //如果为true，则可在LevelListDrawable中使用
    <padding
        android:left="integer"
        android:top="integer"
        android:right="integer"
        android:bottom="integer"/>
    <size //指定大小，一般用在ImageView配合scaleType属性使用
        android:width="integer"
        android:height="integer"/>
    <solid  //填充颜色
        android:color="color"/>
    <stroke //指定边框
        android:width="integer"
        android:color="color" //描边的宽度
        //虚线宽度
        android:dashWidth="integer"
        //虚线间隔宽度
        android:dashGap="integer"/>
</shape>
```

### 绘图技巧
- canvas.save() 保存图层,将之前的绘制所有已经绘制的图像保存起来,让后续的操作就好像在一个新图层上操作一样。
- canvas.restore() 合并图层,将save()之前和save()之后的所有图像合并
- canvas.translate() 画布平移
- canvas.rotate() 画布旋转
- canvas.saveLayer(); canvas.saveLayerAlpha() 方法将一个图层入栈,入栈后的所有操作都在这个图层上
- canvas.restore();canvas.restoreToCount(int) 将一个图层出栈,出栈的时候会把图像绘制到Canvas上

### Bitmap色彩处理(全部都作用于画笔)
#### 使用martix或Android封装好的方法设置色彩
- 色调 hue 

```java
// 0 1 2代表 red green blue ,第二参数代表处理的值
// 可以为RGB三种颜色分量设置不同色调
ColorMatrix hueMatrix = new ColorMatrix();
hueMatrix.setRotate(0,hue1); 
hueMatrix.setRotate(1,hue2);
hueMatrix.setRotate(2,hue3);
```

- 饱和度 saturation

```java
ColorMatrix saturationMatrix = new ColorMatrix();
//saturation 在0~1之间; 饱和度为0时,图像就变灰
saturationMatrix.setSaturation(saturation); 
```

- 亮度 lum

```java
ColorMatrix lumMatrix = new ColorMatrix();
//亮度为0时,图像就变成全黑
lumMatrix.setScale(r,g,b,a);
```

- 混合效果 postConcat

```java
ColorMatrix matrix = new ColorMatrix();
matrix.postConcat(hueMatrix);
matrix.postConcat(saturationMatrix);
matrix.postConcat(lumMatrix);
```

- 设置到paint中去 

```java
//不能再原来的bitmap上进行操作,创建一个bitmap
Bitmap bmp = Bitmap.createBitmap(width, height, Bitmap.Config.ARGB_8888);
Canvas canvas = new Canvas(bmp);
Paint paint = new Paint(Paint.ANTI_ALIAS_FLAG);
paint.setColorFilter(new ColorMatrixColorFilter(imageMatrix));
canvas.drawBitmap(bm, 0, 0, paint);//bm为原bitmap,把bm画在canvas上
```

- ColorMatrix是一个4x5的矩阵 
	- 1 2 3 4行分别代表 r g b a
	- 第5 列,分别各个颜色代表偏移量 
	- 原始矩阵 a[0] a[6] a[12] a[18] 的位置为1,(也是控制亮度的值 setScale)

#### 通过像素点改变色彩

```java
//bm为原图
Bitmap bmp = Bitmap.createBitmap(bm.getWidth(), bm.getHeight(),
                Bitmap.Config.ARGB_8888);
int width = bm.getWidth();
int height = bm.getHeight();
int[] oldPx = new int[width * height];
int[] newPx = new int[width * height];
//获取原图每个像素点,放在oldpx中
bm.getPixels(oldPx, 0, bm.getWidth(), 0, 0, width, height);
//读取每个像素点上的分量
 for (int i = 0; i < width * height; i++) {
 	colorBefore = oldPx[i];
    a = Color.alpha(colorBefore);
    r = Color.red(colorBefore);
    g = Color.green(colorBefore);
    b = Color.blue(colorBefore);
    //通过计算...生成新的newPx数组
    newPx[i] = Color.argb(a, r1, g1, b1);
 }
//设置像素点
bmp.setPixels(newPx, 0, width, 0, 0, width, height);
```

### 图形处理
#### Matrix图形矩阵
- 是一个 3x3的矩阵,初始矩阵  a e i 位置为1,其他位置为0

| a | b | c |
|---|---|---|
| d | e | f |
| g | h | i |
  

 

#### 图形变化的种类
- Translate 
	- c 位置代表 ∆x 轴偏移量  
	- f 位置代表 ∆y 轴偏移量

| 1 | 0 | ∆x |
|---|---|----|
| 0 | 1 | ∆y |
| 0 | 0 | 1  |

- Rotate
	- a 位置 cosθ 
	- b 位置 -sinθ
	- d 位置 sinθ
	- e 位置 cosθ

| cosθ | -sinθ | 0 |
|------|-------|---|
| sinθ | cosθ  | 0 |
| 0    | 0     | 1 |

- Scale 
	- a 位置 x轴放大倍数 x = k1 x x0
	- e 位置 y轴放大倍数 y = k2 x y0
	- matrix.setScale(1f, -1f);垂直翻转

| k1 | 0  | 0 |
|----|----|---|
|  0 | k2 | 0 |
|  0 | 0  | 1 |

- Skew 错切 (横向错切,纵向错切)
	- b 位置 横向错切 x = k1 x y0 + x0 
	- d 位置 纵向错切 y = k2 x x0 + y0
 
| 1  | k1 | 0 |
|----|----|---|
| k2 |  1 | 0 |
| 0  |  0 | 1 |

- pre()和post()提供矩阵的前乘和后乘,来构成混合效果(matrix不满足交换律)

### ProterDuffXfermode (要关闭硬件加速,因为有些模式不支持)
- 先绘制的为Des 后绘制的为Src

```java
//关闭硬件加速
setLayerType(LAYER_TYPE_SOFTWARE, null);
mBitmap = BitmapFactory.decodeResource(getResources(), R.drawable.test1);
mOut = Bitmap.createBitmap(mBitmap.getWidth(),
        mBitmap.getHeight(),
        Bitmap.Config.ARGB_8888);
Canvas canvas = new Canvas(mOut);
mPaint = new Paint(Paint.ANTI_ALIAS_FLAG);
// Dst
canvas.drawRoundRect(0, 0, mBitmap.getWidth(), mBitmap.getHeight(),
        50, 50, mPaint);
mPaint.setXfermode(new PorterDuffXfermode(PorterDuff.Mode.SRC_IN));
// Src
canvas.drawBitmap(mBitmap, 0, 0, mPaint);
mPaint.setXfermode(null);//置空xfermode
```

### Shader 着色器
- LinearGradient 线性
- RadialGradient 光束
- SweepGradient 梯度
- ComposeShader 混合
- CLAMP 拉伸效果,只拉伸最后一个像素
- REPEAT 不断重复效果
- MIRROR 镜像

#### BitmapShader 位图shader

```java
mPaint = new Paint();
mBitmap = BitmapFactory.decodeResource(getResources(), R.drawable.test);
mBitmapShader = new BitmapShader(mBitmap,
        Shader.TileMode.CLAMP, Shader.TileMode.CLAMP);
mPaint.setShader(mBitmapShader);
canvas.drawCircle(300, 200, 200, mPaint);
```

### SurfaceView
模板代码

```java
public class SurfaceViewTemplate extends SurfaceView
        implements SurfaceHolder.Callback, Runnable {
    public SurfaceViewTemplate(Context context) {
        super(context);
        init();
    }
    public SurfaceViewTemplate(Context context, AttributeSet attrs) {
        super(context, attrs);
        init();
    }
    public SurfaceViewTemplate(Context context, AttributeSet attrs, int defStyleAttr) {
        super(context, attrs, defStyleAttr);
        init();
    }
    private void init() {
        mHolder = getHolder();
        mHolder.addCallback(this);
        setFocusable(true);
        setFocusableInTouchMode(true);
        setKeepScreenOn(true);
//        mHolder.setFormat(PixelFormat.OPAQUE);
    }
    private SurfaceHolder mHolder;
    //用于绘制的canvas
    private Canvas mCanvas;
    //子线程标志位
    private boolean mIsDrawing;

    @Override
    public void surfaceCreated(SurfaceHolder holder) {
        mIsDrawing = true;
        new Thread(this).start();
    }
    @Override
    public void surfaceChanged(SurfaceHolder holder, int format, int width, int height) {

    }
    @Override
    public void surfaceDestroyed(SurfaceHolder holder) {
        mIsDrawing = false;
    }
    @Override
    public void run() {
        //在子线程进行绘制
        while (mIsDrawing) {
            //不断的循环绘制
            draw();
        }
    }
    private void draw() {
        try {
            //通过lockCanvas获取当前canvas绘制对象
            //每次获取的canvas对象还是上一次的canvas对象,而不是一个新的对象
            //之前绘图的所有操作将会保留,如果要擦除,可以在绘制前调用drawColor()方法清屏
            mCanvas = mHolder.lockCanvas();
            //绘制背景(清屏)
            mCanvas.drawColor(Color.WHITE);
            //TODO draw something
        } catch (Exception e) {

        } finally {
            if (mCanvas != null) {
                //对绘制内容进行提交
                mHolder.unlockCanvasAndPost(mCanvas);
            }
        }
    }
}
```


## 动画机制
### 视图动画(补间动画) 4种

### 属性动画

####  ObjectAnimator 

```java
ObjectAnimator animator1 = ObjectAnimator.ofFloat(view, "translationX", 300f);
ObjectAnimator animator2 = ObjectAnimator.ofFloat(view, "scaleX", 1f, 0f, 1f);
ObjectAnimator animator3 = ObjectAnimator.ofFloat(view, "scaleY", 1f, 0f, 1f);
AnimatorSet set = new AnimatorSet();
	set.setDuration(1000);
	set.playTogether(animator1, animator2, animator3);//同时调用
	set.start();
```

#### 如果有的属性没有get set怎么办？使用ValueAnimator

```java
ValueAnimator animator = ValueAnimator.ofFloat(0, 100);
animator.setTarget(view);
animator.setDuration(2000).start();
animator.addUpdateListener(new ValueAnimator.AnimatorUpdateListener() {
    @Override
    public void onAnimationUpdate(ValueAnimator animation) {
        Float value = (Float) animation.getAnimatedValue();
    }
});
```

#### PropertyValuesHolder 一个对象的多个属性,同时作用多种动画
```java
 PropertyValuesHolder propertyValuesHolder1 = PropertyValuesHolder.ofFloat("translationX", 300f);
PropertyValuesHolder propertyValuesHolder2 = PropertyValuesHolder.ofFloat("scaleX", 1f,0,1f);
PropertyValuesHolder propertyValuesHolder3 = PropertyValuesHolder.ofFloat("scaleY", 1f,0,1f);
ObjectAnimator.ofPropertyValuesHolder(view, propertyValuesHolder1, propertyValuesHolder2, propertyValuesHolder3)
        .setDuration(3000)
        .start();
```

#### 动画的监听

```java
 //适配器类AnimatorListenerAdapter
 animator.addListener(new Animator.AnimatorListener() {
            @Override
            public void onAnimationStart(Animator animation) {
                
            }

            @Override
            public void onAnimationEnd(Animator animation) {

            }

            @Override
            public void onAnimationCancel(Animator animation) {

            }

            @Override
            public void onAnimationRepeat(Animator animation) {

            }
        });

 ```

### View的animate方法
3.0之后可以使用

### 布局动画
- `android:animateLayoutChanges="true"` 在viewgroup中加上,打开布局动画(只是默认的过渡效果) 
- 通过LayoutAnimationController自定义过渡效果


```java
ScaleAnimation sa = new ScaleAnimation(0, 2, 0, 2);
sa.setDuration(1000);  
//arg1 : 需要的动画 ,arg2 : 每个子view的delay时间
LayoutAnimationController lac = new LayoutAnimationController(sa,0.5f);
//LayoutAnimationController.ORDER_NORMAL 顺序
//LayoutAnimationController.ORDER_RANDOM 随机
//LayoutAnimationController.ORDER_REVERSE 反序
lac.setOrder(LayoutAnimationController.ORDER_NORMAL);
ll.setLayoutAnimation(lac);
```

### 自定义动画
- 继承 Animation
- 覆盖applyTransformation()
- initialize() 进行一些初始化操作

```java
@Override
protected void applyTransformation(float interpolatedTime, Transformation t) {
	//arg1 插值器的时间因子,取值范围 0.0~1.0
	//agr2 矩阵的封装类,通过矩阵来操作
	
}
```
### 5.0之后的activity过渡动画
