# FragmentPagerAdapter和FragementStatePagerAdapter的区别
## 学习目标
- 理解FragmentPagerAdapter和FragementStatePagerAdapter的区别和使用场景
- 学会通过打印日志分析程序的设计机制

## 开堂白
S:

E:

## 课堂内容
* FragmentPagerAdapter:该类内的每一个生成的 Fragment 都将保存在内存之中，因此适用于那些`相对静态的页，数量也比较少`的那种；如果需要处理有很多页，并且数据动态性较大、占用内存较多的情况，应该使用FragmentStatePagerAdapter

* FragmentStatePagerAdapter:该 PagerAdapter 的实现将只保留当前页面，当页面离开视线后，就会被消除，释放其资源，而只是保存页面的状态；而在页面需要显示时，生成新的页面(就像 ListView 的实现一样)。这么实现的好处就是当拥有大量的页面时，不必在内存中占用大量的内存

```  
    FragmentStatePagerAdapter
    对应的fragment会重复的初始化,不会缓存fragment,其实会缓存saveinstance
    FragmentStatePagerAdapter孩子多.而且变换比较大.
    
    左右滑动一圈
    ----------初始化 首页----------
    ----------初始化 应用----------
    ----------初始化 游戏----------
    ----------初始化 专题----------
    ----------初始化 推荐----------
    ----------初始化 分类----------
    ----------初始化 排行----------
    ----------初始化 推荐----------
    ----------初始化 专题----------
    ----------初始化 游戏----------
    ----------初始化 应用----------
    ----------初始化 首页----------

    FragmentPagerAdapter
    对应的Fragment只是初始化了一次,会缓存Fragment对象
    FragmentPagerAdapter 如果孩子比较多.而且经常变换.用FragmentPagerAdapter不好
    
    左右滑动一圈
    ----------初始化 首页----------
    ----------初始化 应用----------
    ----------初始化 游戏----------
    ----------初始化 专题----------
    ----------初始化 推荐----------
    ----------初始化 分类----------
    ----------初始化 排行----------
```

## 重点难点讲解

## 问题和练习
### 问题
1. 导航页适合用 FragmentPagerAdapter 还是 FragmentStatePagerAdapter

### 练习
