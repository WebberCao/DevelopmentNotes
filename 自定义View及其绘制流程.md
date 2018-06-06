#### View绘制流程
当Activity对象被创建完毕后，会将DecorView添加到Window中（Window是对窗口的抽象，DecorView是一个窗口的顶级容器View，其本质是一
个FrameLayout），同时会创建ViewRootImpl（ViewRoot的实现类）对象，并将ViewRootImpl与DecorView建立关联。View的绘制流程从ViewRoot的performTraversals方法开始，经过measure、layout 、draw三大过程完成对一个View的绘制工作。peformTraversal方法内部会调用measure、layout、draw这三个
方，这三个方法内部又分别调用onMeasure、onLayout、onDraw方法。

measure()方法接收两个参数，widthMeasureSpec和heightMeasureSpec，这两个值分别用于确定视图的宽度和高度的规格和大小。MeasureSpec的值由specSize和
specMode共同组成的，其中specSize记录的是大小，specMode记录的是规格。measure过程决定了View的测量宽高，这个过程结束后，就可以通过getMeasuredHeight
和getMeasuredWidth获得View的测量宽高了。

layout过程决定了View在父容器中的位置和View的最终显示宽高。在父容器（这里为LinearLayout）完成了对子元素位置参数（top、left、right、bottom）的获取
后，会调用子元素的layout方法，并把获取到的子元素位置参数传入，从而完成对子元素的layout过程。子元素在自己的layout方法中，也会先完成对自己的布局（确定
四个位置参数），再调用onLayout方法完成对其子View的布局，这样layout过程就沿着View树一层层传了下去。 layout过程完成后，便可以通过getWidth和getHeight
方法获取View的最终显示宽高。 getMeasureWidth()方法在measure()过程结束后就可以获取到了，而getWidth()方法要在layout()过程结束后才能获取到。另
外，getMeasureWidth()方法中的值是通过setMeasuredDimension()方法来进行设置的，而getWidth()方法中的值则是通过视图的坐标位置计算出来的。

绘制视图本身，即调用onDraw()函数。在View中onDraw()是个空函数，也就是说具体的视图都要override该函数来实现自己的显示，而对于ViewGroup则不需要实现该
函数，因为作为容器是“没有内容“的，其包含了多个子view，而子view已经实现了自己的绘制方法，因此只需要告诉子view绘制自己就可以了，也就是下面
的dispatchDraw()方法。


**绘制过程涉及方法**<br>

| 类别  | 方法名  | 描述  |
|:-----:|:--------:|-------|
|测量、布局 | onMeasure | 测量View与Child View的大小 |
|       | onLayout   | 确定Child View的位置  |
|       | onSizeChange | 确定View的大小  |
| 绘制  | onDraw      | 实际绘制View的内容  |
| 事件处理     | onTouchEvent  |   处理屏幕触摸事件|
| 重绘     | invalidate  |  调用onDraw方法，重绘View中变化的部分|
<br>

**Canvas涉及方法**<br>

| 类别        | 方法名           | 描述   |  
| ------------- |:-------------:| -----   |  
| 绘制图形      | drawPoint, drawPoints, drawLine, drawLines, drawRect, drawRoundRect, drawOval, drawCircle, drawArc | 依次为绘制点、直线、矩形、圆角矩形、椭圆、圆、扇形 |
| 绘制文本      | drawText, drawPosText, drawTextOnPath |    依次为绘制文字、指定每个字符位置绘制文字、根据路径绘制文字|
| 画布变换      | translate, scale, rotate, skew |   依次为平移、缩放、旋转、倾斜（错切） |
| 画布裁剪      | clipPath, clipRect, clipRegion |   依次为按路径、按矩形、按区域对画布进行裁剪 |
<br>

**Paint涉及方法**<br>

| 类别        | 方法名           | 描述  |
| ------------- |:-------------:| -----   | 
| 颜色      | setColor, setARGB, setAlpha | 依次为设置画笔颜色、透明度 |
| 类型      | setStyle |   填充(FILL),描边(STROKE),填充加描边(FILL_AND_STROKE) |
| 抗锯齿      | setAntiAlias |   画笔是否抗锯齿 |
| 字体大小      | setTextSize |   设置字体大小 |
| 字体测量      | getFontMetrics()，getFontMetricsInt() |   返回字体的测量，返回值依次为float、int |
| 文字宽度      | measureText |   返回文字的宽度 |
| 文字对齐方式      | setTextAlign |   左对齐(LEFT),居中对齐(CENTER),右对齐(RIGHT) |
| 宽度      | setStrokeWidth |   设置画笔宽度 |
| 笔锋      | setStrokeCap |   默认(BUTT),半圆形(ROUND),方形(SQUARE) |
<br>

**Path涉及方法**<br>

| 类型  | 方法名| 描述 |
| ------------- |:-------------:| ------------- |
| 添加路径 | addArc, addCircle, addOval, addPath, addRect, addRoundRect, arcTo | 依次为添加圆弧、圆、椭圆、路径、矩形、圆角矩形、圆弧|
| 移动起点 | moveTo | 移动起点位置，仅对之后路径产生影响 |
| 移动终点 | setLastPoint | 移动上一次的终点位置，对前后的路径都会产生影响|
| 直线 | lineTo | 增加一条道指定点的直线 |
| 贝塞尔 | quadTo, cubicTo | 二阶、三阶贝塞尔曲线 |
| 闭合路径 | close | 路径终点连接到起点|
| 逻辑运算 | op | A\B(DIFFERENCE), A∩B(INTERSECT), B\A(REVERSE_DIFFERENCE), A∪B(UNION), A⊕B(XOR)|
| 替换路径 | set | 用新的路径替换当前路径 |
| 重置 | reset, rewind| 清除path使它为空，清除path但保留内部的数据结构 |
| 计算边界 | computeBounds| 计算路径的矩形边界 |
| 闭合方向 | Direction | 顺时针方向闭合Path(CW),逆时针方向闭合Path(CCW) |
<br>

**一个完整的绘制过程会依次绘制以下几个内容：**<br>
1. 背景
2. 主体（onDraw()）
3. 子 View（dispatchDraw()）
4. 滑动边缘渐变和滑动条
5. 前景







