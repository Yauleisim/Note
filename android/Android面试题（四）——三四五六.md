Android面试题（四）——三四五六

===

1. 四大组件
   - Activity：Activity是Android程序与用户交互的窗口。
   - service：后台服务于Activity。
   - Content Provider：是Android提供的第三方应用数据的访问方案，可以派生Content Provider类，对外提供数据，可以像数据库一样进行选择排序，屏蔽内部数据的存储细节，向外提供统一的接口模型，大大简化上层应用，对数据的整合提供了更方便的途径。
   - BroadCast Receiver：接受一种或者多种Intent作触发事件，接受相关消息，做一些简单处理，转换成一条Notification，统一了Android的事件广播模型。
2. 六大布局
   - FrameLayout：所有东西依次都放在左上角，会重叠。
   - LinearLayout：线性布局，每一个LinearLayout里面又可分为垂直布局（android:orientation="vertical"）和水平布局（android:orientation="horizontal" ）。当垂直布局时，每一行就只有一个元素，多个元素依次垂直往下；水平布局时，只有一行，每一个元素依次向右排列。
   - AbsoluteLayout：绝对布局用X,Y坐标来指定元素的位置，这种布局方式也比较简单，但是在屏幕旋转时，往往会出问题，而且多个元素的时候，计算比较麻烦。
   - RelativeLayout：相对布局可以理解为某一个元素为参照物，来定位的布局方式
   - TableLayout：表格布局，每一个TableLayout里面有表格行TableRow，TableRow里面可以具体定义每一个元素。每向TableLayout中添加一个TableRow就代表一行；每向TableRow中添加一个一个子组件就表示一列。
   - GridLayout：把整个容器划分为rows × columns个网格，每个网格可以放置一个组件。
3. 五种数据存储方式：
   - SharedPreferences:用来存储一些简单配置信息的一种机制 。只能在同一个包内使用，不能在不同的包之间使用。 
   - 文件存储：提供了openFileInput()和openFileOutput()方法来读取设备上的文件。 
   - SQLite数据库存储 
   - 网络存储
   - 使用ContentProvider存储数据 
4. 三大动画
   - 帧动画：允许你实现像播放幻灯片一样的效果。
   - 补间动画：是对某个View进行一系列的动画的操作，包括淡入淡出（Alpha），缩放（Scale），平移（Translate），旋转（Rotate）四种模式。
   - 属性动画：修改控件的属性值实现的动画。