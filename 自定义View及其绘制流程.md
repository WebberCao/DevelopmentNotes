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

onMeasure       测量View与Child View的大小

onLayout	      确定Child View的位置

onSizeChanged	  确定View的大小

onDraw	        实际绘制View的内容

onTouchEvent	  处理屏幕触摸事件

invalidate	    调用onDraw方法，重绘View中变化的部分

