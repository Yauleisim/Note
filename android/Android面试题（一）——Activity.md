Android面试题（一）——Activity

===

1. Activity生命周期

   - 官方文档图

     ![官网文档的生命周期图](https://images0.cnblogs.com/i/426802/201406/060009291302389.png)

   - 生命周期解析图

     ![生命周期图解析](https://images2015.cnblogs.com/blog/877390/201703/877390-20170304103100126-1196400911.jpg)

   - 补充：

     1. Activity运行时按下**HOME键**：

        onSaveInstanceState -->onPause --> onStop  --> onRestart-->onStart--->onResume

     2. onSaveInstanceState:

        当某个activity变得**容易**被系统销毁时，该activity的onSaveInstanceState就会被执行，除非该activity是被用户主动销毁的，例如当用户按BACK键的时候。这里的容易被销毁，指的是**可能**被系统销毁的时候（强调的是可能)。

        可能调用的时机：

        1、当用户按下HOME键时。

        2、长按HOME键，选择运行其他的程序时。

        3、按下电源按键（关闭屏幕显示）时。

        4、从activity A中启动一个新的activity时。

        5、屏幕方向切换时，例如从竖屏切换到横屏时。

     3. onRestoreInstanceState:

        onSaveInstanceState方法和onRestoreInstanceState方法“不一定”是成对的被调用的，onRestoreInstanceState被调用的前提是，activity**确实**被系统销毁了，而如果仅仅是停留在有这种可能性的情况下，则该方法不会被调用。

   - 切换横竖屏的生命周期：

     1. 不设置Activity的android:configChanges时，切屏会重新调用各个生命周期，切横屏时会执行一次，切竖屏时会执行两次
     2. 设置Activity的android:configChanges="orientation"时，切屏还是会重新调用各个生命周期，切横、竖屏时只会执行一次
     3. 设置Activity的android:configChanges="orientation|keyboardHidden"时，切屏不会重新调用各个生命周期，只会执行onConfigurationChanged方法

2. Activity启动模式

   - 设置方式：

     - 通过ActivityManifest的android:launchMode设置

     - 在Intent中指定，如：

        ```java
         Intent intent = new Intent();
         intent.setClass(context, MainActivity.class);
         intent.addFlags(Intent.FLAG_ACTIVITY_NEW_TASK);
         context.startActivity(intent);
        ```

     - 代码设置会优先于清单文件设置 。

     - 第一种方式无法为Activity直接指定 **FLAG_ACTIVITY_CLEAR_TOP** 标识，另外一种方式无法为Activity指定 **singleInstance** 模式。 

   - standard：是Android的默认启动模式。这种模式下，Activity可以有**多个实例**，每次启动Activity，无论任务栈中是否已经有这个Activity的实例，系统都会**创建一个新的**Activity实例，

   - SingleTop:SingleTop模式和standard模式非常相似，主要区别就是当一个singleTop模式的Activity已经位于任务栈的**栈顶**，再去启动它时，**不会再创建新的实例**,如果不位于栈顶，就会创建新的实例。

      - 当一个Activity已经在栈顶，但依然有可能启动它，而你又不想产生新的Activity实例，此时就可以用singleTop模式。例如，一个搜索Activity，可以输入搜索内容，也可以产生搜索结果，此时就可以用singleTop模式，不会用户每次搜索都会产生一个实例。
      - 对于每次启动Activity，虽然系统不会调用onCreat(),但会调用onNewIntent，我们可以在这个函数做相应的处理。

   - SingleTask:SingleTask模式的Activity在同一个Task内只有一个实例，如果Activity已经位于栈顶，系统不会创建新的Activity实例，和singleTop模式一样。但Activity已经存在但不位于栈顶时，系统就会把该Activity移到栈顶，并把它上面的activity出栈。

   - SingleInstance:singleInstance模式也是单例的，但和singleTask不同，singleTask只是**任务栈内**单例，系统里是可以有多个singleTask Activity实例的，而singleInstance Activity在**整个系统里**只有一个实例，启动一singleInstanceActivity时，系统会创建一个新的任务栈，并且这个任务栈只有他一个Activity。

   - 启动模式图：

![启动模式图](http://img.blog.csdn.net/20170303202108396?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvSVRlcm1lbmc=/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)



3. 如何传递数据

   - ```java
     intent.putExtra(key,value);
     ```

     value不但可以使基本数据类型，还可以是对象的引用，但这个对象对应的类必须实现序列化接口（即Serializable或Parcelable )

     - Serializable和Pacelable接口的区别简单来讲：Serializable 基于反射，运行时占用内存大; Pacelable 基于分解，执行效率高；

4. 如何设置成窗口样式

   - 在AndroidManifest.xml文件当中设置当前activity的一个属性（系统自带的属性）：

     ```java
     android:theme="@android:style/Theme.Dialog"
     ```