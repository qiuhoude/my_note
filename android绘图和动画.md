
# Android绘图与动画笔记

## 绘图机制和处理


### 屏幕尺寸信息

|  密度  |     ldpi    |    mdpi   |     hdpi    |   xhdpi   |   xxhdpi  |
|--------|-------------|-----------|-------------|-----------|-----------|
| 密度值 | 120         | 160       | 240         | 320       | 480       |
| 分辨率 | 240x320     | 320x480   | 480x800     | 720x1280  | 1080x1920 |
| 公式   | 1dp = 3/4px | 1dp = 1px | 1dp = 1.5px | 1dp = 2px | 1dp = 2px |
 

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

### ProterDuffXfermode

### Shader 着色器
#### BitmapShader 位图shader
- 先绘制的为Des 后绘制的为Src

## 动画机制