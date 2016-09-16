# PSTS功能拓展_修改Indicator为三角形
## 学习目标
- 会使用 path 构建三角形
- 理解三角形 indicator 的坐标计算公式

## 开堂白
S:

E:

## 课堂内容
在 onDraw 方法中使用 path 绘制三角形代替以前绘制矩形的功能
```java
Path path = new Path();
float x1 = (lineRight - lineLeft) / 2 + lineLeft;
float y1 = height - indicatorHeight;
//三角形边长
int triangleWidth = 30;
float x2 = x1 + triangleWidth / 2;
float y2 = height;
float x3 = x1 - triangleWidth / 2;
float y3 = height;

path.moveTo(x1, y1);
path.lineTo(x2, y2);
path.lineTo(x3, y3);
path.lineTo(x1, y1);
canvas.drawPath(path, rectPaint);
```

## 重点难点讲解

## 问题和练习
### 问题

### 练习
1. 给定一个矩形的坐标为(left, top, right, bottom), 给出这个巨形中间的正三角形三个点的坐标
2. 自定义一个 View 使用 path 和 canvas 绘制一个凹字
