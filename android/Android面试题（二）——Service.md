Android面试题（二）——Service
===

1. Service启动方法

   1. startService：主要用于启动一个服务执行后台任务，不进行通信。停止服务使用stopService。

      ![startService开启服务](http://upload-images.jianshu.io/upload_images/944365-c9d086267869945c.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

   2. bindService：该方法启动的服务可以进行通信。停止服务使用unbindService。

      ![bindService绑定服务](http://upload-images.jianshu.io/upload_images/944365-ca62abafd7815297.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

      - IPC：扩展Binder类、使用Messager、AIDL

   3. startService同时也bindService：停止服务应同时使用stopService与unbindService。

      ![示意图](http://upload-images.jianshu.io/upload_images/944365-b42335ad20daed14.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

      

   

