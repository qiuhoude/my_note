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


### onLayout
- 决定子view该放在什么位置
- `childView.layout(cl,ct,cr,cb)`来设置子view的位置
- 参数的意思
	- cl：子View左边界到父view左边界的距离
	- ct：子View上边界到父view上边界的距离
	- cr：子View右边界到父view左边界的距离
	- cb：子View下边距到父View上边界的距离


