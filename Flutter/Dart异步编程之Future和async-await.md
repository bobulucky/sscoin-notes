####Event-Looper
Dart是单线程模型，也就没有了所谓的主线程/子线程之分。
Dart也是Event-Looper以及Event-Queue的模型，所有的事件都是通过EventLooper的依次执行。

Dart的Event Looper就是：
* 从EventQueue中获取Event
* 处理Event
* 直到EventQueue为空

![Event Queue](https://upload-images.jianshu.io/upload_images/1941624-5cd24aaf768a8b97.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/362/format/webp)

这些Event包括了用户输入，点击，Timer，文件IO等

![Event Type](https://upload-images.jianshu.io/upload_images/1941624-c156a82a7aec374e.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/440/format/webp)

####单线程模型


####什么是future
Dart语言中的Future表示在将来某时获取一个值的方式。当一个返回Future的函数被调用的时候，做了两件事：
1、 函数把自己放入队列和返回一个未完成的Future对象
2、 之后当值可用时，Future带着值变成完成状态
3、 为了获得Future的值，有两种方式：使用async和await使用Future的接口
