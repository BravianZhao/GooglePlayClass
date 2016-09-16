# PSTS属性效果和开源项目源码分析
## 学习目标
- 总结PagerSlidingTabStrip默认自定义属性效果
- 记住学习开源项目的方法
- 掌握查看开源项目源码的方法，能够熟练的读懂一般的开源 View 项目的源码
- 分析 PagerSlidingTabStrip 抛出 `ViewPager does not have adapter instance.` 的原因
- 分析为什么 ViewPager 和 PagerSlidingTabStrip 绑定之后，要给 ViewPager 设置 onPageChangeListener 只能通过 PagerSlidingTabStrip 设置

## 开堂白
S:

E:

## 课堂内容
### 查看PagerSlidingTabStrip默认属性效果
* `pstsIndicatorColor` 滑动条颜色
* `pstsUnderlineColor` 下划线颜色
* `pstsDividerColor` 分割线颜色
* `pstsIndicatorHeight` 滑动条高度
* `pstsUnderlineHeight` 下划线高度
* `pstsDividerPadding` 分割线顶部底部padding
* `pstsTabPaddingLeftRight` tab 的左右 padding
* `pstsScrollOffset` 选中 tab 的滚动偏移量
* `pstsTabBackground` tab 的背景色
* `pstsShouldExpand` 是否展开把所有 tab 都显示出来，每个 tab 都一样宽
* `pstsTextAllCaps` tab 上都转化为大写字母

### 针对开源项目学习的步骤

1. 学会使用-->学会集成到自己项目中
2. 学会看源码
3. 修改源码
4. 自己写一些开源项目

### PagerSlidingTabStrip源码的查看
#### 1. 看继承结构 
```java
PagerSlidingTabStrip extends HorizontalScrollView
```

#### 2. 看构造方法
看构造方法-->一定会调用的方法

```java
public PagerSlidingTabStripExtends(Context context) {
    this(context, null);
}

public PagerSlidingTabStripExtends(Context context, AttributeSet attrs) {
    this(context, attrs, 0);
}

public PagerSlidingTabStripExtends(Context context, AttributeSet attrs, int defStyle) {
    super(context, attrs, defStyle);

    setFillViewport(true);
    setWillNotDraw(false);

    tabsContainer = new LinearLayout(context);
    tabsContainer.setOrientation(LinearLayout.HORIZONTAL);
    tabsContainer.setLayoutParams(new LayoutParams(LayoutParams.MATCH_PARENT, LayoutParams.MATCH_PARENT));
    addView(tabsContainer);

    DisplayMetrics dm = getResources().getDisplayMetrics();

    scrollOffset = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, scrollOffset, dm);
    indicatorHeight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, indicatorHeight, dm);
    underlineHeight = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, underlineHeight, dm);
    dividerPadding = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, dividerPadding, dm);
    tabPadding = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, tabPadding, dm);
    dividerWidth = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_DIP, dividerWidth, dm);
    tabTextSize = (int) TypedValue.applyDimension(TypedValue.COMPLEX_UNIT_SP, tabTextSize, dm);

    // get system attrs (android:textSize and android:textColor)
    TypedArray a = context.obtainStyledAttributes(attrs, ATTRS);

    tabTextSize = a.getDimensionPixelSize(0, tabTextSize);
    tabTextColor = a.getColor(1, tabTextColor);

    a.recycle();

    // get custom attrs
    a = context.obtainStyledAttributes(attrs, R.styleable.PagerSlidingTabStrip);

    indicatorColor = a.getColor(R.styleable.PagerSlidingTabStrip_pstsIndicatorColor, indicatorColor);
    underlineColor = a.getColor(R.styleable.PagerSlidingTabStrip_pstsUnderlineColor, underlineColor);
    dividerColor = a.getColor(R.styleable.PagerSlidingTabStrip_pstsDividerColor, dividerColor);
    indicatorHeight = a.getDimensionPixelSize(R.styleable.PagerSlidingTabStrip_pstsIndicatorHeight, indicatorHeight);
    underlineHeight = a.getDimensionPixelSize(R.styleable.PagerSlidingTabStrip_pstsUnderlineHeight, underlineHeight);
    dividerPadding = a.getDimensionPixelSize(R.styleable.PagerSlidingTabStrip_pstsDividerPadding, dividerPadding);
    tabPadding = a.getDimensionPixelSize(R.styleable.PagerSlidingTabStrip_pstsTabPaddingLeftRight, tabPadding);
    tabBackgroundResId = a.getResourceId(R.styleable.PagerSlidingTabStrip_pstsTabBackground, tabBackgroundResId);
    shouldExpand = a.getBoolean(R.styleable.PagerSlidingTabStrip_pstsShouldExpand, shouldExpand);
    scrollOffset = a.getDimensionPixelSize(R.styleable.PagerSlidingTabStrip_pstsScrollOffset, scrollOffset);
    textAllCaps = a.getBoolean(R.styleable.PagerSlidingTabStrip_pstsTextAllCaps, textAllCaps);

    a.recycle();

    rectPaint = new Paint();
    rectPaint.setAntiAlias(true);
    rectPaint.setStyle(Style.FILL);

    dividerPaint = new Paint();
    dividerPaint.setAntiAlias(true);
    dividerPaint.setStrokeWidth(dividerWidth);

    defaultTabLayoutParams = new LinearLayout.LayoutParams(LayoutParams.WRAP_CONTENT, LayoutParams.MATCH_PARENT);
    expandedTabLayoutParams = new LinearLayout.LayoutParams(0, LayoutParams.MATCH_PARENT, 1.0f);

    if (locale == null) {
        locale = getResources().getConfiguration().locale;
    }
}
```

1. 这里 PagerSlidingTabStrip 的构造方法最后都会调用到 3 个参数的构造方法
2. `setWillNotDraw(false);` 表示该 view 需要被绘制；`setWillNotDraw(true)` 表示 view 不需要被绘制，`onDraw` 方法会被跳过，不会执行
3. `addView(tabsContainer);` 表示 PagerSlidingTabStrip 的儿子节点是 tabsContainer 这个 LinearLayout
4. `TypedValue.applyDimension()` 表示某种单位(dp|sp|pt 等等)表示的尺寸的在本设备上的像素值大小，除 px 外，同一个单位的同一个尺寸在不同屏幕像素密度的设备上表示的 px 大小不同

#### 3. 看外界调用的方法
```java
mTabs.setViewPager(viewPager)
```

```java
public void setViewPager(ViewPager pager) {
    this.pager = pager;

    if (pager.getAdapter() == null) {
        throw new IllegalStateException("ViewPager does not have adapter instance.");
    }
    //绑定了一个OnPageChangeListener
    pager.setOnPageChangeListener(pageListener);

    notifyDataSetChanged();
}
```

1. `throw new IllegalStateException("ViewPager does not have adapter instance.");` 表示设置 ViewPager 的时候，ViewPager 必须已经设置过 Adapter 对象
2. `pager.setOnPagerChangeListener(pageListener);` 一个 ViewPager 对象只能有一个 PagerChange 监听器，所以想要给 ViewPager 添加其他PagerChange监听器，只能通过监听 PagerSlidingTabStrip；v4 包升级后增加了一个 `addOnPageChangeListener()` 方法，可以给 ViewPager 设置多个 PagerChange 监听器

```java
public void notifyDataSetChanged() {
    //删掉线性布局中的所有子 view
    tabsContainer.removeAllViews();
    tabCount = pager.getAdapter().getCount();

    //根据 pager 数量添加 TextTab 或者 IconTab
    for (int i = 0; i < tabCount; i++) {
        if (pager.getAdapter() instanceof IconTabProvider) {
            addIconTab(i, ((IconTabProvider) pager.getAdapter()).getPageIconResId(i));
        } else {
            addTextTab(i, pager.getAdapter().getPageTitle(i).toString());
        }
    }

    updateTabStyles();

    //View 树布局完成会调用 listener 中的 onGlobalLayout 方法
    getViewTreeObserver().addOnGlobalLayoutListener(new OnGlobalLayoutListener() {
        @SuppressWarnings("deprecation")
        @SuppressLint("NewApi")
        @Override
        public void onGlobalLayout() {
            //移除监听器
            if (Build.VERSION.SDK_INT < Build.VERSION_CODES.JELLY_BEAN) {
                getViewTreeObserver().removeGlobalOnLayoutListener(this);
            } else {
                getViewTreeObserver().removeOnGlobalLayoutListener(this);
            }

            currentPosition = pager.getCurrentItem();
            scrollToChild(currentPosition, 0);
        }
    });
}
```

```java
private void addTextTab(final int position, String title) {
    TextView tab = new TextView(getContext());
    tab.setText(title);
    tab.setGravity(Gravity.CENTER);
    tab.setSingleLine();

    addTab(position, tab);
}

private void addTab(final int position, View tab) {
    tab.setFocusable(true);
    tab.setOnClickListener(new OnClickListener() {
        @Override
        public void onClick(View v) {
            pager.setCurrentItem(position);
        }
    });

    tab.setPadding(tabPadding, 0, tabPadding, 0);
    tabsContainer.addView(tab, position, shouldExpand ? expandedTabLayoutParams : defaultTabLayoutParams);
}
```

```java
private void updateTabStyles() {
    //遍历 tabsContainer 中所有子 view，设置 view 文字的样式
    for (int i = 0; i < tabCount; i++) {
        View v = tabsContainer.getChildAt(i);
        //设置 view 的背景
        v.setBackgroundResource(tabBackgroundResId);

        if (v instanceof TextView) {
            TextView tab = (TextView) v;
            //设置 tab 上文字的大小
            tab.setTextSize(TypedValue.COMPLEX_UNIT_PX, tabTextSize);
            //设置 tab 上文字的字体
            tab.setTypeface(tabTypeface, tabTypefaceStyle);
            //设置 tab 上文字的颜色
            tab.setTextColor(tabTextColor);

            // setAllCaps() is only available from API 14, so the upper case is made manually if we are on a
            // pre-ICS-build
            if (textAllCaps) {
                if (Build.VERSION.SDK_INT >= Build.VERSION_CODES.ICE_CREAM_SANDWICH) {
                    tab.setAllCaps(true);
                } else {
                    tab.setText(tab.getText().toString().toUpperCase(locale));
                }
            }
        }
    }
}
```

#### 4. 看覆写的方法
onMeasure onDraw onLayout
```java
@Override
protected void onDraw(Canvas canvas) {
    super.onDraw(canvas);

    if (isInEditMode() || tabCount == 0) {
        return;
    }

    final int height = getHeight();

    // draw indicator line，绘制滑动条
    rectPaint.setColor(indicatorColor);

    // default: line below current tab
    View currentTab = tabsContainer.getChildAt(currentPosition);
    float lineLeft = currentTab.getLeft();
    float lineRight = currentTab.getRight();

    // if there is an offset, start interpolating left and right coordinates between current and next tab
    if (currentPositionOffset > 0f && currentPosition < tabCount - 1) {
        View nextTab = tabsContainer.getChildAt(currentPosition + 1);
        final float nextTabLeft = nextTab.getLeft();
        final float nextTabRight = nextTab.getRight();

        lineLeft = (currentPositionOffset * nextTabLeft + (1f - currentPositionOffset) * lineLeft);
        lineRight = (currentPositionOffset * nextTabRight + (1f - currentPositionOffset) * lineRight);
    }
    //绘制矩形的indicator
    canvas.drawRect(lineLeft, height - indicatorHeight, lineRight, height, rectPaint);

    // draw underline，绘制底部线
    rectPaint.setColor(underlineColor);
    canvas.drawRect(0, height - underlineHeight, tabsContainer.getWidth(), height, rectPaint);

    // draw divider，绘制分割线
    dividerPaint.setColor(dividerColor);
    for (int i = 0; i < tabCount - 1; i++) {
        View tab = tabsContainer.getChildAt(i);
        canvas.drawLine(tab.getRight(), dividerPadding, tab.getRight(), height - dividerPadding, dividerPaint);
    }
}
```

## 重点难点讲解

## 问题和练习
### 问题
1. 列举分析开源项目源码的一般步骤
2. `TypedValue.applyDimension()` 方法有什么作用
3. PagerSlidingTabStrip 抛出 `ViewPager does not have adapter instance.` 的原因是什么
4. 说说 ViewPager 和 PagerSlidingTabStrip 绑定之后，给 ViewPager 设置 onPageChangeListener 会出现什么问题

### 练习


