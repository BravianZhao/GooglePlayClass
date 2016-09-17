# PSTS功能拓展_添加自定义属性
## 学习目标
- 给自定义控件添加自定义属性

## 开场白
S:

E:

## 课堂内容
添加 4 个自定义属性
    + 默认时候的字体颜色
    + 默认时候的字体大小
    + 选中时候的字体颜色
    + 选中时候的字体大小

### 1. 定义 PagerSlidingTabStripExtends 类
从 PagerSlidingTabStrip copy 一份源码

### 2. 声明自定属性的名字和取值格式
在 res/values/attrs.xml 文件中声明自定属性的名字和取值格式

```xml
<declare-styleable name="PagerSlidingTabStrip">
    <attr name="pstsIndicatorColor" format="color"/>
    <attr name="pstsUnderlineColor" format="color"/>
    <attr name="pstsDividerColor" format="color"/>
    <attr name="pstsIndicatorHeight" format="dimension"/>
    <attr name="pstsUnderlineHeight" format="dimension"/>
    <attr name="pstsDividerPadding" format="dimension"/>
    <attr name="pstsTabPaddingLeftRight" format="dimension"/>
    <attr name="pstsScrollOffset" format="dimension"/>
    <attr name="pstsTabBackground" format="reference"/>
    <attr name="pstsShouldExpand" format="boolean"/>
    <attr name="pstsTextAllCaps" format="boolean"/>

    <!--默认时候的字体颜色-->
    <attr name="pstsTabTextColor" format="color"/>
    <!--默认时候的字体大小-->
    <attr name="pstsTabTextSize" format="dimension"/>
    <!--选中时候的字体颜色-->
    <attr name="pstsSelectedTabTextColor" format="color"/>
    <!--选中时候的字体大小-->
    <attr name="pstsSelectedTabTextSize" format="dimension"/>
</declare-styleable>
```

### 3. 添加 2 个属性
在 PagerSlidingTabStripExtends 类中用来保存额外的自定义属性

```java
/*--------------- add begin ---------------*/
private int selectedTabTextSize = 12;
private int selectedTabTextColor = 0xFF666666;
/*--------------- add end ---------------*/
```

### 4. 取出额外属性
在 PagerSlidingTabStripExtends 构造方法中从布局文件中取出设置的额外属性 

```java
/*--------------- add begin ---------------*/
//取出额外添加的4个自定义属性
tabTextColor = a.getColor(R.styleable.PagerSlidingTabStrip_pstsTabTextColor, tabTextColor);
tabTextSize = a.getDimensionPixelSize(R.styleable.PagerSlidingTabStrip_pstsTabTextSize, tabTextSize);

selectedTabTextColor = a.getColor(R.styleable.PagerSlidingTabStrip_pstsSelectedTabTextColor, selectedTabTextColor);
selectedTabTextSize = a.getDimensionPixelSize(R.styleable.PagerSlidingTabStrip_pstsSelectedTabTextSize, selectedTabTextSize);
/*--------------- add end ---------------*/
```

### 5. 使用属性值修改 text 的样式
在 updateTabStyles() 方法中修改 text 的样式
```java
/*--------------- add begin ---------------*/
if (i == curSelectedPosition) {
    tab.setTextSize(TypedValue.COMPLEX_UNIT_PX, selectedTabTextSize);
    tab.setTypeface(tabTypeface, tabTypefaceStyle);
    tab.setTextColor(selectedTabTextColor);
}
/*--------------- add end ---------------*/
```
添加成员变量 curSelectedPosition
```java
private int curSelectedPosition;//当前ViewPager选中页面所对应的position
```
在 ViewPager 页面变化监听器中修改 curSelectedPosition 的值，更新 text 的样式
```java
@Override
public void onPageSelected(int position) {
    if (delegatePageListener != null) {
        delegatePageListener.onPageSelected(position);
    }
    /*--------------- add begin ---------------*/
    curSelectedPosition = position;
    updateTabStyles();
    /*--------------- add end ---------------*/
}
```

### 6. 使用扩展之后的自定义控件
在 res/values/color.xml 文件中增加颜色定义
```xml
<color name="tab_indicator_selected">#6666FF</color>
<color name="tab_text_selected">#6666FF</color>
<color name="tab_text_normal">#333333</color>
```
在布局文件中使用自定义控件和设置属性
```xml
<!--PagerSlidingTabStrp-->
<com.astuetz.PagerSlidingTabStripExtends
    android:id="@+id/main_tabs"
    android:layout_width="match_parent"
    android:layout_height="48dp"
    itheima:pstsIndicatorColor="@color/tab_indicator_selected"
    itheima:pstsSelectedTabTextColor="@color/tab_text_selected"
    itheima:pstsSelectedTabTextSize="18sp"
    itheima:pstsTabTextColor="@color/tab_text_normal"
    itheima:pstsTabTextSize="16sp" />
```

## 重点难点讲解

## 问题和练习
### 问题

### 练习
1. 自定义一个带有下列自定义属性的 PagerSlidingTabStrip
    + 默认时候的字体颜色
    + 默认时候的字体大小
    + 选中时候的字体颜色
    + 选中时候的字体大小