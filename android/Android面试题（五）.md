Android面试题（五）

===

1. 进程间通信

   1. Bundle
   2. 文件共享
   3. Messenger 
   4. AIDL
   5. ContentProvider
   6. Socket

   ![进程间通信](https://img-blog.csdn.net/20170605011532312?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvdTAxMTI0MDg3Nw==/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

2. handler机制

   ![示意图](https://upload-images.jianshu.io/upload_images/1824194-a947dcc5b22f69fb.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/561)

3. View绘制流程

   - View的整个绘制流程可以分为以下三个阶段：

     1. measure: 判断是否需要重新计算View的大小，需要的话则计算；
     2. layout: 判断是否需要重新计算View的位置，需要的话则计算；
     3. draw: 判断是否需要重新绘制View，需要的话则重绘制。

     ![View绘制流程图](https://upload-images.jianshu.io/upload_images/2397836-19c08de6439514a7.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

4. 事件分发机制

   - 事件的分发过程由三个很重要的方法来共同完成：dispatchTouchEvent、onInterceptTouchEvent和onTouchEvent
     1. dispatchTouchEvent：用来进行事件的分发。如果事件能够被传递给当前View，那么此方法一定会被调用，返回结果受到当前View的onTouchEvent和下级View的dispatchTouchEvent方法影响，表示是否消耗当前事件。
     2. onInterceptTouchEvent：会在dispatchTouchEvent方法内部调用，用来判断是否拦截某个事件。
     3. onTouchEvent：在dispatchTouchEvent方法中调用，用来处理事件，返回结果表示是否消耗当前事件。

5. lv与rv

   - 区别：

     1. 两者的缓存层级不同，ListView只有两层，RecycleView有四级缓存。
     2. LV全局刷新，RV局部刷新。
     3. RecyclerView多了一些LayoutManager工作，但实现了布局效果多样化。

   - rv优化：

     1. 问题：在RecyclerView列表滚动时，如果item中需要网络加载的图片资源过多或过大，会造成掉帧卡顿的问题，用户体验不是很好，尤其是在滚动非常快的情况下。

        方案：当惯性滚动停下来时，才进行网络请求加载图片并进行绘制。

     2. 

6. fragment
   - 使用场景：本来针对平板大屏而设计，后来应用于各种切换效果。

   - 生命周期

     ![fragment生命周期](https://images2015.cnblogs.com/blog/945877/201611/945877-20161123093212096-2032834078.png)

   - 与Activity交互

     1. 接口回调：在Fragment中定义一个内部回调接口，再让包含该Fragment的Activity类实现这个接口，这样Fragment就能够调用这个回调方法，将数据传给Activity中。
     2. 广播
     3. 在Activity中创建Bundle数据包，并调用Fragment对象的setArguments（Bundle bundle）方法。Fragment类中调用getArguments（）方法就能够获得这个Bundle包。
     4. EventBus