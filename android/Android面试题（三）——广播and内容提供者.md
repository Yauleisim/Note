Android面试题（三）——广播and内容提供者
===

1. 广播注册方式

   1. 在代码当中注册（动态）

      - 注册的方法是registerReceiver（receiver，filter）（用Activity的实例来调用）
      - 取消注册的方法：unregisterReceiver（receiver），

   2. 在AndroidManifest当中进行注册（静态）

      ```java
      <receiver android:name=".broadcastreceiver.net_monitor.InternetStaticBroadCastReceiver">
          <intent-filter>
              <!--<action android:name="ConnectivityManager.CONNECTIVITY_ACTION"/>//这样写是不对的-->
              <action android:name="android.net.conn.CONNECTIVITY_CHANGE"/>//此处必需指定action，否则监听不到
          </intent-filter>
      </receiver>
      ```

   3. 区别：

      1. 第一种不是常驻型广播，也就是说广播跟随程序的生命周期。
      2. 第二种是常驻型，也就是说当应用程序关闭后，如果有信息广播来，程序也会被系统调用自动运行。

   4. 优先级：动态（谁先注册高）>静态。

2. 广播内部通信实现机制

   - 具体实现流程要点粗略概括如下：

     1. 广播接收者BroadcastReceiver通过Binder机制向AMS(Activity Manager Service)进行注册；
     2. 广播发送者通过binder机制向AMS发送广播；
     3. AMS查找符合相应条件（IntentFilter/Permission等）的BroadcastReceiver，将广播发送到BroadcastReceiver（一般情况下是Activity）相应的消息循环队列中；
     4. 消息循环执行拿到此广播，回调BroadcastReceiver中的onReceive()方法。

   - Binder机制：Binder是Android系统中进程间通讯（IPC）的一种方式。

     ![Binder简单原理](https://upload-images.jianshu.io/upload_images/3985563-5ff2c4816543c433.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/700)

3. 广播生命周期：生命周期非常短暂，在接收到广播的时候创建，onReceive方法结束后销毁。

4. 广播种类

   1. 普通广播：开发者自身定义intent的广播（最常用） 。
   2. 系统广播：Android中内置的广播。
   3. 有序广播：发送出去的广播被广播接收者按照先后顺序接收。
   4. 本地广播：一种局部广播，广播的发送者和接收者都同属于一个App。安全性高 & 效率高。

5. contentProvider如何实现数据共享

   - 它将共享的数据进行包装，然后对外暴露接口，外界的应用程序就能通过接口来访问被封装的数据，从而达到数据共享的目的。