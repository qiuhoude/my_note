## android 自定义控件

### 定义和获取属性
##### values/attrs.xml的属性说明

```xml
<declare-styleable name="CustomView">  
    <!-- 引用资源 -->  
    <attr name="image" format="reference" />  
    <!-- 颜色 -->  
    <attr name="text_color" format="color" />  
    <!-- 布尔值 -->  
    <attr name="text_display" format="boolean" />  
    <!-- 尺寸大小 -->  
    <attr name="temp1" format="dimension" />  
    <!-- 浮点值 -->  
    <attr name="temp2" format="float" />  
    <!-- 整型值 -->  
    <attr name="temp3" format="integer" />  
    <!-- 字符串 -->  
    <attr name="text" format="string" />  
    <!-- 百分比 -->  
    <attr name="alpha" format="fraction" />  
    <!-- 枚举 -->  
    <attr name="text_align" format="integer">  
        <enum name="left" value="0" />  
        <enum name="right" value="1" />  
        <enum name="center" value="2" />  
    </attr>  
    <!-- 位运算 -->  
    <attr name="text_optimize" format="integer">  
        <flag name="anti" value="0x001" />  
        <flag name="dither" value="0x002" />  
        <flag name="linear" value="0x004" />  
    </attr> 
    <!-- 位运算的使用 -->
</declare-styleable>  
```

##### 获取自定义CustomView的属性 

- 在构造方法中获取属性,一种是通过`TypedArray`
- 另一种有个构造参数`AttributeSet attr`进行获取。两者的区别在于,如果布局文件中如值是引用类型的(例如`@string/name`),`AttributeSet`要先获取最终的字符串，需要第一步拿到`id`，第二部去解析`id`,而使用`TypedArray`可以直接获取最终的字符串。
- 代码片段

```java
//通过TypedArray获取属性
public CustomView(Context context, AttributeSet attrs) {
	super(context, attrs);
	TypedArray ta = context.obtainStyledAttributes(attrs, R.styleable.CustomView);
	String text = ta.getString(R.styleable.CustomView_text);
	int temp3 = ta.getInteger(R.styleable.CustomView_temp3,-1);
	ta.recycle();
}
//通过TypedArray获取属性,并且要自定义主题的
public CustomView(Context context, AttributeSet attrs, int defStyle) {
	super(context, attrs, defStyle);
	TypedArray ta = context.getTheme().obtainStyledAttributes(attrs,
				R.styleable.CustomView, defStyle, 0);
	int n = ta.getIndexCount();
	for(int i=0; i < n; i++){
		int attr = a.getIndex(i);
		switch (attr) {
			case R.styleable.CustomView_text:
				text = ta.getString(attr);
				break;
			case R.styleable.CustomView_temp3:
				temp3 = ta.getInteger(attr,-1);
				break;
		}
	}
	ta.recycle();
}
//通过AttributeSet获取属性
public CustomView(Context context, AttributeSet attrs) {
	super(context, attrs);
	int count = attrs.getAttributeCount();
	for (int i = 0; i < count; i++) {
	    String attrName = attrs.getAttributeName(i);
	    String attrVal = attrs.getAttributeValue(i);
	}
	//如果是引用类型数据先获取id
	int widthDimensionId =  attrs.getAttributeResourceValue(0, -1);
	float width = getResources().getDimension(widthDimensionId));
}

```
- `declare-styleable`标签可以不用写,直接在CustomView定义`static final int[] mAttr = {android.R.attr.text, R.attr.testAttr}`,通过`TypedArray ta = context.obtainStyledAttributes(attrs, mAttr);`,然后根据数组下标获取对应属性值`String text = ta.getString(0)`
- `attrs.xml`里面的`declare-styleable`以及item，android会根据其在R.java中生成一些常量方便我们使用(aapt干的)，本质上，我们可以不声明`declare-styleable`仅仅声明所需的属性即可。

### 测量onMeasure
- `int specMode = MeasureSpec.getMode(widthMeasureSpec)`获取模式
- `int specSize = MeasureSpec.getSize(widthMeasureSpec)`获取尺寸
- `MeasureSpec`其实就是封装了size和mode
- `setMeasuredDimension(width,height)` 设置自己计算后的宽高(需要根据模式来设置不同宽高,可以参考`getDefaultSize()`来写)
- 测量的时候要加上内边距`getPadding()`和,要不然在布局文件中设置padding不起作用
- 在ViewGroup`measureChild()`计算子view的大小,然后用`child.getMeasuredWidth()`获取子view计算的宽高
- 如果当前ViewGroup有自己LayoutParams,子view可以通过`MarginLayoutParams param = child.getLayoutParams()`,如果要使margin起作用，就要在子view计算后的宽高后加上margin`int childWidth = child.getMeasuredWidth()+param.leftMargin+param.rightMargin`
- `measureChildren()`会测量所有的子View，如果View的状态不是GONE就调用measureChild去进行下一步的测量
- 调用ViewGroup`measureChildWithMargins()`也可以上margin的值
- 


##### MeasureSpec的3种模式
- `EXACTLY` 一般是设置了__具体的值__或者是MATCH_PARENT
- `AT_MOST` 表示子布局限制在一个最大值内，一般为WARP_CONTENT
- `UNSPECIFIED` 父不没有对子施加任何约束，一般出现在AadapterView的item的heightMode中、ScrollView的childView的heightMode中；此种模式比较少见,UNSPECIFIED在源码中的处理和EXACTLY一样。当View的宽高值设置为0的时候或者没有设置宽高时，模式为UNSPECIFIED 

##### ViewGroup和LayoutParams之间的关系
>当在LinearLayout中写childView的时候，可以写layout_gravity，layout_weight属性；在RelativeLayout中的childView有layout_centerInParent属性，却没有layout_gravity，layout_weight，这是为什么呢？这是因为每个ViewGroup需要指定一个LayoutParams，用于确定支持childView支持哪些属性，比如LinearLayout指定LinearLayout.LayoutParams等。如果大家去看LinearLayout的源码，会发现其内部定义了LinearLayout.LayoutParams，在此类中，你可以发现weight和gravity的身影。

- ViewGroup定义LayoutParams,子view可以使用，比如margin属性

```java
@Override
protected LayoutParams generateDefaultLayoutParams() {
	return new MarginLayoutParams(LayoutParams.MATCH_PARENT,
			LayoutParams.MATCH_PARENT);
}

@Override
public LayoutParams generateLayoutParams(AttributeSet attrs) {
	return new MarginLayoutParams(getContext(), attrs);
}

@Override
protected LayoutParams generateLayoutParams(LayoutParams p) {
	return new MarginLayoutParams(p);
}
```

##### getMeasuredHeight()与getHeight的区别
- getMeasuredHeight()是实际View的大小,getHeight控件在屏幕中大小,当view不超过屏幕，两者相等
- 如果view超出屏幕,getMeasuredHeight()等于getHeight()加上屏幕之外没有显示的大小

#### 如何获取未加载完成的view的宽高
- 使用`view.measure(0,0)`方法可以主动通知系统去测量，然后就可以直接使用它获取宽高,`getMeasuredHeight()`  
- 通过拿到`ViewTreeObserver`增加监听器  

```java
view.getViewTreeObserver()
.addOnGlobalLayoutListener(new OnGlobalLayoutListener() {

    @Override
    public void onGlobalLayout() {
        //移除监听器
        view.getViewTreeObserver().removeGlobalOnLayoutListener(this);
        //直接可以获取宽高
        view.getHeight();
    }
});
```
- 还可以是用`view.post(Runnable) `来处理布局加载完成之后的操作


### onLayout
- 决定子view该放在什么位置
- `childView.layout(cl,ct,cr,cb)`来设置子view的位置
- 参数的意思
	- cl：子View左边界到父view左边界的距离
	- ct：子View上边界到父view上边界的距离
	- cr：子View右边界到父view左边界的距离
	- cb：子View下边距到父View上边界的距离



### ViewGroup的其他方法
*  `onFinishInflate()` 从xml加载组件后调用
*  `onSizeChanged()` 组件大小改变时调用

### 获取坐标
#### view提供的
- `getTop()` view自身的顶部到__父布局顶部的距离__
- `getLeft()`view自身的左边到__父布局左边的距离__
- `getRight()` view自身的右边到__父布局左边的距离__
- `getBottom()`view自身的底部到__父布局顶部的距离__

#### MotionEvent提供的
- `getX()` 获取点击事件距离控件__左边__的距离 ，视图坐标
- `getY()` 获取点击事件距离控件__顶边__的距离 ，视图坐标
- `getRawX()`获取点击事件距离整个屏幕左边的距离,绝对坐标
- `getRawY()`获取点击事件距离整个屏幕顶边的距离,绝对坐标


### 在ViewGroup中,让子view实现滑动方法(7种)
* `layout(l,t,r,b);` viewgroup中的方法
* `offsetTopAndBottom(offset)和offsetLeftAndRight(offset);` view中的方法
* `LayoutParams` 通过`getLayoutParams()` 获取，然后`layoutParams.leftMargin = getLeft()+ offsetX`,最后`setLayoutParams()`设置进去
* `scrollTo`(具体位置),`scrollBy`(相对当前位置) view中的方法 注意：单独的view调用是没效果的(必须外层包裹layout),滚动的并不是viewgroup内容本身，而是它的矩形框里面的内容进行滚动,移动是瞬间。  
  
平滑移动  
* scrollview 中有 `smoothScrollTo`,`smoothScrollBy`进行平滑的移动
* `Scroller`可以模拟一个执行流程
* `ViewDragHelper` 中 `smoothSlideViewTo()` 也可以进行平滑移动. 


### Scroller 辅助类
#### 1 构造器
```java
//传入Interpolator补间器,可以实现动画的变化率，比如匀速变化 先加速后减速
Scroller(Context context, Interpolator补间器,可以实现动画的变化率，比如先快后慢 interpolator)
```
#### 绘制方法
`startScroll(int startX, int startY, int dx, int dy, int duration)` 滚动之前的基本设置,并没有滚动view  

`computeScrollOffset()` 计算滚动，如果动画持续时间小于我们设置的滚动持续时间mDuration,进去switch的SCROLL_MODE，然后根据Interpolator来计算出在该时间段里面移动的距离,赋值给mCurrX， mCurrY， 所以该方法的作用是，计算在0到mDuration时间段内滚动的偏移量，并且判断滚动是否结束，true代表还没结束，false则表示滚动  

`view`的`computeScroll()`该方法是滑动的控制方法,在绘制View时，会在`draw()`过程调用该方法  

View滚动的实现原理，我们先调用Scroller的`startScroll()`方法来进行一些滚动的初始化设置，然后迫使View进行绘制，我们调用View的invalidate()或postInvalidate()就可以重新绘制View，绘制View的时候会触发`computeScroll()`方法，我们重写computeScroll()，在computeScroll()里面先调用Scroller的`computeScrollOffset()`方法来判断滚动有没有结束，如果滚动没有结束我们就调用scrollTo()方法来进行滚动，该scrollTo()方法虽然会重新绘制View,但是我们还是要手动调用下invalidate()或者postInvalidate()来触发界面重绘，重新绘制View又触发computeScroll()，所以就进入一个循环阶段，这样子就实现了在某个时间段里面滚动某段距离的一个平滑的滚动效果  
  
使用

### VelocityTracker 和 ViewConfiguration 辅助类(可以自定义ScrollView)
- 获取 `mVelocityTracker = VelocityTracker.obtain();`, `mViewConfiguration = ViewConfiguration.get(context);`
- `mViewConfiguration.getScaledTouchSlop()` 获得能够进行手势滑动的距离,手的移动要大于这个距离才开始移动控件。如果小于这个距离就不触发移动控件，如viewpager就是用这个距离来判断用户是否翻页
- `mMaximumVelocity = mViewConfiguration.getScaledMaximumFlingVelocity()`获得允许执行一个fling手势动作的最大速度值
- `mMinimumVelocity = mViewConfiguration.getScaledMinimumFlingVelocity()`获得允许执行一个fling手势动作的最小速度值
- `mVelocityTracker.computeCurrentVelocity(1000, mMaximumVelocity);`之后调用`mVelocityTracker.getYVelocity()`获取当前y轴的速度  
  
自定义控件，平滑移动的思路：  
参考：[泡网](http://www.jcodecraeer.com/a/anzhuokaifa/androidkaifa/2012/1114/558.html)  
> 初始化  

```java
private void init(Context context) {  
    mScroller = new Scroller(getContext());  
    setFocusable(true);  //是否聚焦
    setDescendantFocusability(FOCUS_AFTER_DESCENDANTS);  
    setWillNotDraw(false);  
    final ViewConfiguration configuration = ViewConfiguration.get(context);  
    mTouchSlop = configuration.getScaledTouchSlop();  
    mMinimumVelocity = configuration.getScaledMinimumFlingVelocity();  
    mMaximumVelocity = configuration.getScaledMaximumFlingVelocity();
    }
```

> 申明一个用来处理滑动操作的方法fling(int velocityY)

```java
public void fling(int velocityY) {  
    if (getChildCount() > 0) {  
            mScroller.fling(getScrollX(), getScrollY(), 0, velocityY, 0, 0, 0,  
                            maxScrollEdge);  
            final boolean movingDown = velocityY > 0;  
            //awakenScrollBars(mScroller.getDuration());//动画开始的延时,当参数startDelay为0时动画将立刻开始，其实就是一个延迟的作用 
            invalidate();  
    }  
}
```

> VelocityTracker获取资源和释放资源的封装

```java
private void obtainVelocityTracker(MotionEvent event) {  
    if (mVelocityTracker == null) {  
            mVelocityTracker = VelocityTracker.obtain();  
    }  
    mVelocityTracker.addMovement(event);  
      
}  
      
     
private void releaseVelocityTracker() {  
    if (mVelocityTracker != null) {
            mVelocityTracker.clear();  
            mVelocityTracker.recycle();  
            mVelocityTracker = null;  
    }  
}
```

> onTouchEvent(MotionEvent event)方法的重写

```java
public boolean onTouchEvent(MotionEvent event) {
    if (event.getAction() == MotionEvent.ACTION_DOWN && event.getEdgeFlags() != 0) {  
        return false;  
    }  
    obtainVelocityTracker(event);  
    final int action = event.getAction();  
    final float x = event.getX();  
    final float y = event.getY();  
    switch (action) {  
        case MotionEvent.ACTION_DOWN:  
            Log.d(TAG, "ACTION_DOWN#currentScrollY:" + getScrollY()  
                            + ", mLastY:" + mLastY);  
            if (!mScroller.isFinished()) {  
                    mScroller.abortAnimation();  
            }  
            mLastY = y;  
            return true;  
    case MotionEvent.ACTION_MOVE:  
            final int deltaY = (int) (mLastY - y);  
            if (deltaY < 0) {  
                if (getScrollY() > 0) {  
                    scrollBy(0, deltaY);  
                }   
            } else if (deltaY > 0) {  
                mIsInEdge = getScrollY() <= childTotalHeight - height;  
                if (mIsInEdge) {  
                    scrollBy(0, deltaY);  
                }  
            }  
            mLastY = y;  
            break;  
    case MotionEvent.ACTION_UP:  
            final VelocityTracker velocityTracker = mVelocityTracker;  
            velocityTracker.computeCurrentVelocity(1000, mMaximumVelocity);  
            int initialVelocitY = (int) velocityTracker.getYVelocity();  
            if ((Math.abs(initialVelocitY) > mMinimumVelocity)  
                            && getChildCount() > 0) {  
                    fling(-initialVelocitY);  
            }  
            releaseVelocityTracker();  
            break;  
     case MotionEvent.ACTION_CANCEL:
            mIsInEdge = false;
            recycleVelocityTracker();
            if (!mScroller.isFinished()) {
                mScroller.abortAnimation();
            }
            break;
    }  
    return super.onTouchEvent(event);  
}
```

> 重写computeScroll()

```java
 if (mScroller.computeScrollOffset()) {//判断滑动是否完成
        scrollTo(0, mScroller.getCurrY());
        postInvalidate();
    }
```

1. 首先我们通过`VelocityTracker`、`ViewConfiguration`类得到一些惯性滑动所必须的变量，比如手势离开屏幕时的初始速度，允许进行手势操作的最小距离以及允许手势操作的速度边界值；
2. 创建Scroller的对象，使用它的fling方法供我们控制界面滑动使用；
3. 重写onTouchEvent方法，当我们用手指在屏幕上来回滑动时此时执行的是scrollBy方法来刷新界面，当手指离开屏幕，此时就要开始执行`ACTION_UP`后面的操作了；
    1. 通过对手指离开屏幕时的速度进行判断是否能够进行惯性滑动操作，
    2. 如果能够执行那么就使用Scroller类的fling方法启动滑动动画，
    3. 这时需要调用一下invalidate()方法来间接的调用computeScroll方法，
    4. 在computeScroll方法中对Scroller的动画是否执行完成做了判断
    5. 如果动画没有完成(mScroller.computeScrollOffset() == true)那么就使用scrollTo方法对mScrollX、mScrollY的值进行重新计算刷新界面，
    6. 调用postInvalidate()方法重新绘制界面，
    7. postInvalidate()方法会调用invalidate()方法，
    8. invalidate()方法又会调用computeScroll方法，
    9. 就这样周而复始的相互调用，直到mScroller.computeScrollOffset()返回false才会停止界面的重绘动作
 


### ViewDragHelper 自定义ViewGroup帮助类

* 创建实例
```java
mDragger = ViewDragHelper.create(this, 1.0f, new ViewDragHelper.Callback(){}`
```

> 创建实例需要3个参数，第一个就是当前的ViewGroup，第二个sensitivity，主要用于设置touchSlop:
`helper.mTouchSlop = (int) (helper.mTouchSlop * (1 / sensitivity));` 可见传入越大，mTouchSlop的值就会越小。第三个参数就是Callback，在用户的触摸过程中会回调相关方法，后面会细说。

* 触摸相关方法

```java
 @Override
public boolean onInterceptTouchEvent(MotionEvent event)
{
	//自定义的viewgroup的交给ViewDragHelper去决定是否拦截事件
    return mDragger.shouldInterceptTouchEvent(event);
}

@Override
public boolean onTouchEvent(MotionEvent event)
{
    mDragger.processTouchEvent(event);
    return true;
}
```

* ViewDragHelper.callback相关方法
>ViewDragHelper拦截看自定义viewgroup事件，所以需要回调来控制

> 常用回调的方法

* `tryCaptureView` 如何返回ture则表示可以捕获该view，你可以根据传入的第一个view参数决定哪些可以捕获

```java
//根据返回结果决定当前child是否可以拖拽
// child 当前被拖拽的View
// pointerId 区分多点触摸的id
@Override
public boolean tryCaptureView(View child, int pointerId) {
    Log.d(TAG, "tryCaptureView: " + child);
    return true;
};
```
* `onViewCaptured` 当captureview被捕获时回调

* `clampViewPositionHorizontal`,`clampViewPositionVertical` 根据建议值 修正将要移动到的(横向)位置,__此时view没有发生真正的移动__

```java
//  根据建议值 修正将要移动到的(横向)位置  
// 此时没有发生真正的移动
public int clampViewPositionHorizontal(View child, int left, int dx) {
    // child: 当前拖拽的View
    // left 新的位置的建议值, dx 位置变化量
    // left = oldLeft + dx;
    Log.d(TAG, "clampViewPositionHorizontal: " 
            + "oldLeft: " + child.getLeft() + " dx: " + dx + " left: " +left);
    return left;
}
```

* 如果子view是`clickable`,说明可以消耗事件,使用`clampViewPositionHorizontal`对child移到边界控制就不起作用，主要是因为，如果子View不消耗事件，那么整个手势（DOWN-MOVE*-UP）都是直接进入onTouchEvent，在onTouchEvent的DOWN的时候就确定了captureView。如果消耗事件，那么就会先走onInterceptTouchEvent方法，判断是否可以捕获，而在判断的过程中会去判断另外两个回调的方法:`getViewHorizontalDragRange`和`getViewVerticalDragRange`只有这两个方法返回大于0的值才能正常的捕获。重写下面这两个方法

```java
//返回拖拽的范围, 不对拖拽进行真正的限制. 仅仅决定了动画执行速度
@Override
public int getViewHorizontalDragRange(View child)
{
     return getMeasuredWidth()-child.getMeasuredWidth();
}
```

* `onViewPositionChanged` 当captureview的位置发生改变时回调(进行更新状态, 伴随动画, 重绘界面),__此时,View已经发生了位置的改变__

```java
// changedView 改变位置的View
// left 新的左边值
// dx 水平方向变化量
public void onViewPositionChanged(View changedView, int left, int top,int dx, int dy) {}
```

* `onViewReleased` 手指释放的时候回调,(执行动画)

```java
// View releasedChild 被释放的子View 
// float xvel 水平方向的速度, 向右为+正
// float yvel 竖直方向的速度, 向下为+
public void onViewReleased(View releasedChild, float xvel, float yvel) {}
```

* `onEdgeDragStarted`在边界拖动时回调,需要设置才能回调`mDragger.setEdgeTrackingEnabled(ViewDragHelper.EDGE_LEFT)`
* `onViewDragStateChanged` 当ViewDragHelper状态发生变化时回调（IDLE,DRAGGING,SETTING[自动滚动时]）
* `onEdgeLock` true的时候会锁住当前的边界，false则unLock。
* `getOrderedChildIndex` 改变同一个坐标（x,y）去寻找captureView位置的方法。（具体在：findTopChildUnder方法中）

总结回调
```
shouldInterceptTouchEvent：

DOWN:
    getOrderedChildIndex(findTopChildUnder)
    ->onEdgeTouched

MOVE:
    getOrderedChildIndex(findTopChildUnder)
    ->getViewHorizontalDragRange & 
      getViewVerticalDragRange(checkTouchSlop)(MOVE中可能不止一次)
    ->clampViewPositionHorizontal&
      clampViewPositionVertical
    ->onEdgeDragStarted
    ->tryCaptureView
    ->onViewCaptured
    ->onViewDragStateChanged

processTouchEvent:

DOWN:
    getOrderedChildIndex(findTopChildUnder)
    ->tryCaptureView
    ->onViewCaptured
    ->onViewDragStateChanged
    ->onEdgeTouched
MOVE:
    ->STATE==DRAGGING:dragTo
    ->STATE!=DRAGGING:
        onEdgeDragStarted
        ->getOrderedChildIndex(findTopChildUnder)
        ->getViewHorizontalDragRange&
          getViewVerticalDragRange(checkTouchSlop)
        ->tryCaptureView
        ->onViewCaptured
        ->onViewDragStateChanged
```
* `ViewDragHelper` 中 `smoothSlideViewTo()` 也可以进行平滑移动.因为里面维护了一个Scroller;所以viewgroup需要重写`computeScroll()`方法,并在里面调用ViewDragHelper的`continueSettling(true)`(每次执行都会调用回调的`onViewPositionChanged()方法`),会返回Boolean类型判断是否继续动画,true代码动画为执行完成。

```java
 // 触发一个平滑动画
if (mDragHelper.smoothSlideViewTo(mMainContent, finalLeft, finalTop)) {
    // 返回true代表还没有移动到指定位置, 需要刷新界面.
    // 参数传this(child所在的ViewGroup)
    ViewCompat.postInvalidateOnAnimation(this);
}

 @Override
public void computeScroll() {
    super.computeScroll();
    //持续平滑动画 (高频率调用)
    if (mDragHelper.continueSettling(true)){
        //  如果返回true, 动画还需要继续执行
        ViewCompat.postInvalidateOnAnimation(this);
    }
}
```

### ViewCompat 
view更新动画重绘 ViewCompat.postInvalidateOnAnimation(view)



###  事件
#### 触控事件 MotionEvent 常用事件
- `ACTION_DOWN` 单点按下
- `ACTION_UP` 单点触摸离开
- `ACTION_MOVE` 触摸点移动动作
- `ACTION_CANCEL` 触摸动作取消
- `ACTION_OUTSIDE` 触摸动作超出边界
- `ACTION_POINTER_DOWN` 多点触摸按下动作
- `ACTION_POINTER_UP` 多点离开动作

#### 事件机制总结
[参考1](http://www.cnblogs.com/Jackwen/p/5239035.html)  
[参考2](http://www.cnblogs.com/sunzn/archive/2013/05/10/3064129.html)  



|           Touch事件相关方法           |   功能   | Activity | ViewGroup | View |
|---------------------------------------|----------|----------|-----------|------|
|   dispatchTouchEvent(MotionEvent ev)  | 事件分发 |   Yes    |    Yes    | Yes  |
| onInterceptTouchEvent(MotionEvent ev) | 事件拦截 |    No    |    Yes    |  No  |
|      onTouchEvent(MotionEvent ev)     | 事件响应 |   Yes    |    Yes    | Yes  |
  
activity和 最小单位的view(比如TextView) 是没有事件拦截的  

- `dispatchTouchEvent`   
当Touch事件发生时，Activity的dispatchTouchEvent()方法会以隧道方式从根节点依次往下传递直到最内层子节点，或在中间某一节点中由于某一条件停止传递)将事件传递给最外层View的dispatchTouchEvent()方法，并由该View的dispatchTouchEvent()方法对事件进行分发。
    - `return true` ：事件会分发给当前View并由dispatchTouchEvent()方法进行消费，同时事件会停止向下传递。
    - `return false` ：将事件返还给当前View的上一级的onTouchEvent()进行消费。(这个上一级可能是Activity，也可能是父View)
    - `return super.dispatchTouchEvent(ev)` ：事件会自动的分发给当前View的onInterceptTouchEvent方法。
    - 注意，View响应dispatchTouchEvent()和onInterceptTouchEvent()的前提是可以向该View中添加子View，也就是说该View有子节点才谈得上能分发和拦截。

- `onInterceptTouchEvent`  只有viewgrop才会有
在外层View的dispatchTouchEvent()方法返回super.dispatchTouchEvent(ev)时，事件会自动的分发给当前View的onInterceptTouchEvent()方法。
    - `return true` ：将对事件进行拦截，并将拦截到的事件交由当前View的onTouchEvent()进行处理。
    - `return false` ：将对事件进行放行，当前View上的事件会被传递到子View 上，再由子View的dispatchTouchEvent()来继续对这个事件进行分发。
    - ` return super.onInterceptTouchEvent(ev)` ：默认不拦截返回false

- `onTouchEvent`
    - `return false` ：事件将会从当前View向上传递，并且都是由上层View的onTouchEvent()来接收，如果传递到上层的onTouchEvent()也返回false，那么这                       个事件就会"消失"，而且接收不到下一次事件。
    - `return true` ：接收并消费掉该事件。
    - `return super.onTouchEvent(ev) `：默认处理事件的逻辑和return false相同,但是CLICKABLE的view默认是消耗的  

#### Activity对Touch事件的处理
当Touch事件发生生，最先被触发的是Activity的dispatchTouchEvent()函数，再由这个函数触发根节点的dispatchTouchEvent()。如果想让Activity不响应触摸事件，可以直接重写这个函数

#### view对Touch事件的处理
- `dispatchTouchEvent` 会先判断view时候有`OnTouchListener`,如果有就先执行`onTouch`函数,当`onTouch`返回false才会继续执行view的`onTouchEvent`
- `onTouchEvent` onLongClick是 ACTION_DOWN 进行处理(根据时间长短来判断是否时长按,默认500毫秒)   
- 在`MotionEvent.ACTION_UP`时执行才会onClick
- OnLongClickListener返回true消耗事件并拦截OnClickListener事件
- 如果一个控件是可点击的，那么点击该控件时，onTouchEvent的返回值必定是true

#### ViewGroup对Touch事件的处理
- `onInterceptTouchEvent`
    - 实现这个方法来拦截所有触摸屏移动事件。这可以让你在事件被分配给孩子的时候看到事件，并在任意时刻获取当前手势的所有权。
    - 使用这个功能需要特别注意，因为它与View.onTouchEvent有着相当复杂的相互作用，并且使用它还需要以正确的方式实现这个方法。
    - 顺序:
    - (1) ViewGroup会在这里接收到DOWN事件，这个DOWN事件会被这个ViewGroup的一个子View处理掉，或者给这个ViewGroup自己的onTouchEvent函数进行处理。那就意味着ViewGroup应该实现onTouchEvent()函数并且返回true, 这样你就会继续看到手势的后续事件(而不是寻找父View来处理)。
    - (2) 通过从ViewGroup自己的onTouchEvent()返回true, ViewGroup的onInterceptTouchEvent()中将不会再收到任何后续事件，即后续事件不用再经过onInterceptTouchEvent()了，所有触摸的处理都像正常情况一样发生在onTouchEvent里了。
    - (3) 只要ViewGroup从这个函数返回false，接下来的每个事件(包括最后的UP)都会首先传递到这里，然后再传递给目标View的onTouchEvent()。
    - (4) 如果ViewGroup从这个函数返回true，说明从子view拦截了事件，并将它们通过onTouchEvent()传递给这个ViewGroup。当前目标View将接收到相同事件，但是Action为ACTION_CANCEL，并且没有进一步消息在这里传递。
- `dispatchTouchEvent` 进行事件分发
- `getParent().requestDisallowInterceptTouchEvent(true)` 可以请求父布局不拦截自己的onTouchEvent事件

### 如果要让自定义view有保存数据
- 需要重写`onSaveInstanceState()` 和 `onRestoreInstanceState()` 并且还要保存父类的数据

```java
protected Parcelable onSaveInstanceState() {
    Bundle bundle = new Bundle();
    bundle.putParcelable(INSTANCE_STATUS,super.onSaveInstanceState());//保持父类数据
    bundle.putFloat(STATUS_ALPHA,mAlpha);
    return bundle;
}
protected void onRestoreInstanceState(Parcelable state) {
    if(state instanceof  Bundle){
        Bundle bundle = (Bundle) state;
        mAlpha = bundle.getFloat(STATUS_ALPHA);
        super.onRestoreInstanceState(bundle.getParcelable(INSTANCE_STATUS));
    }else {
        super.onRestoreInstanceState(state);
    }
}
```

- 要给自定义view,在xml中加上id



### ListView 的技巧
##### 分割线 
```xml 
android:divider="@android:color/darker_gray"
android:dividerHeight="10dp"
```
##### 滚动条设置
```xml
android:scrollbars="none"
```
##### item点击效果
```xml
<!-- 取消点击效果 -->
android:listSelector="#00000000"
<!-- or 使用Android原始的透明颜色 -->
android:listSelector="@android:color/transparent"
```
##### item显示位置
```java
listView.setSelection(N);
//或者平滑的移动
listView.smoothScrollToPosition(position);
listView.smoothScrollByOffset(offset);
listView.smoothScrollBy(distance,duration);
```
 