# 使用 PagerslidingTabStrip
## 学习目标
- 在布局文件中使用 PagerslidingTabStrip
- 使用 Butterknife 和其插件进行控件注入

## 开场白
S:

E:

## 课堂内容

### 提交 PagerslidingTabStrip Module 到 svn 仓库
#### 1. 添加 build 目录、*.iml 文件到忽略列表
如果把 build 目录添加到版本控制忽略列表出现如下错误，原因是 build 目录中已经存在 .svn 文件夹，可以先把 build 目录中的 .svn 文件夹全部删掉或者直接把 build 目录删掉，编译生成 build 目录，然后再添加到忽略列表

![](img/PagerslidingTabStrip 022.png )

#### 2. 删除 library.iml
一个 module 只需要一个 *.iml 文件，这个 iml 文件和 module 名字相同， libary.iml 是修改名字之前的 iml 文件，所以可以删掉

![](img/PagerslidingTabStrip 024.png )

#### 3. 点击提交按钮

### 在布局中使用 PagerslidingTabStrip
#### 1. 添加对 PagerslidingTabStrip 库的依赖
给 GooglePlay module 添加对 PagerslidingTabStrip 库的依赖，有 2 种方式

- 依赖 maven 服务器上的 PagerslidingTabStrip 库
- 依赖 PagerslidingTabStrip module

[步骤详情](10.01添加对 PagerslidingTabStrip 库的依赖.html)

#### 2. 在布局文件中使用 PagerslidingTabStrip
一般的 UI 效果图中，PagerslidingTabStrip 在布局中放在 ViewPager 上边，在 MainActivity 的布局中修改内容区域布局

```xml
<!--内容区域-->
    <LinearLayout
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:orientation="vertical"
        >
        <!--PagerSlidingTabStrp-->
        <com.astuetz.PagerSlidingTabStripExtends
            android:id="@+id/main_tabs"
            android:layout_width="match_parent"
            android:layout_height="48dp"
            itheima:pstsIndicatorColor="@color/tab_indicator_selected"
            itheima:pstsSelectedTabTextColor="@color/tab_text_selected"
            itheima:pstsSelectedTabTextSize="18sp"

            itheima:pstsTabTextColor="@color/tab_text_normal"
            itheima:pstsTabTextSize="16sp"
            ></com.astuetz.PagerSlidingTabStripExtends>

        <!--viewPager-->
        <android.support.v4.view.ViewPager
            android:id="@+id/main_viewpager"
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            ></android.support.v4.view.ViewPager>
    </LinearLayout>
```

### 通过 ButterKnife 和插件初始化控件
#### 1. 添加对 ButterKnife jar 包的依赖
[步骤详情](10.02添加对 ButterKnife jar 包的依赖.html)

#### 2. 使用插件自动产生 View 绑定
使用 ButterKnife Zelezny 插件自动产生 View 绑定/注入

1. 给 Android Studio 安装 ButterKnife Zelezny 插件
2. 把光标放在代码中布局文件id位置
3. 右键 -> Generate -> Generate Butterknife Injects

[步骤详情](10.03使用 ButterKnife Zelezny 插件自动产生 View 绑定.html)

```java
@InjectView(R.id.main_tabs)
PagerSlidingTabStripExtends mMainTabs;

@InjectView(R.id.main_viewpager)
ViewPager mMainViewpager;

@Override
protected void onCreate(Bundle savedInstanceState) {
    super.onCreate(savedInstanceState);
    setContentView(R.layout.activity_main);
    ButterKnife.inject(this);

    ......
}
```

## 重点难点讲解

## 问题和练习
### 问题

### 练习
1. 在 GooglePlay 的 MainActivity 的布局中使用 PagerslidingTabStrip 和 ViewPager
2. 使用 ButterKnife 和 ButterKnife Zelezny 自动注入 View 依赖