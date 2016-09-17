# FramgentPagerAdapter使用小问题
## 学习目标
- 理解 FramgentPagerAdapter 的 instanteItem 方法
- 解决FramgentPagerAdapter使用notifyDataSetChanged无效问题

## 开场白
S:

E:

## 课堂内容
FramgentPagerAdapter使用notifyDataSetChanged无效问题演示分析

```java
public Object instantiateItem(ViewGroup container, int position) {
    if(this.mCurTransaction == null) {
        this.mCurTransaction = this.mFragmentManager.beginTransaction();
    }

    long itemId = this.getItemId(position);
    String name = makeFragmentName(container.getId(), itemId);
    Fragment fragment = this.mFragmentManager.findFragmentByTag(name);
    if(fragment != null) {
        this.mCurTransaction.attach(fragment);
    } else {
        fragment = this.getItem(position);
        this.mCurTransaction.add(container.getId(), fragment, makeFragmentName(container.getId(), itemId));
    }

    if(fragment != this.mCurrentPrimaryItem) {
        fragment.setMenuVisibility(false);
        fragment.setUserVisibleHint(false);
    }

    return fragment;
}

public void destroyItem(ViewGroup container, int position, Object object) {
    if(this.mCurTransaction == null) {
        this.mCurTransaction = this.mFragmentManager.beginTransaction();
    }

    this.mCurTransaction.detach((Fragment)object);
}
```

## 重点难点讲解

## 问题和练习
### 问题

### 练习
1. 自己设计一个小案例演示 FramgentPagerAdapter使用notifyDataSetChanged无效问题











