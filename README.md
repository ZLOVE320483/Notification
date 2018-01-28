# Notification

## 说明
  本项目展示了常用的Android系统自带的Notification，以及总结了一些自己在使用notification的过程中遇到的坑。
  
    首先来看一下常见的notification type：
    
    public static final int TYPE_NORMAL = 1;  // 普通通知
    public static final int TYPE_PROGRESS = 2;  // 下载进度的通知
    public static final int TYPE_BIG_TEXT = 3;  // BigTextStyle通知
    public static final int TYPE_INBOX = 4;  // InboxStyle
    public static final int TYPE_BIG_PICTURE = 5;  // BigPictureStyle
    public static final int TYPE_HANGUP = 6;  // hangup横幅通知
    
### 一、普通通知
![普通通知](https://github.com/ZLOVE320483/Notification/blob/master/img/simple.png)
>
    NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
    //为了版本兼容  选择V7包下的NotificationCompat进行构造
    NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
    //Ticker是状态栏显示的提示
    builder.setTicker("简单Notification");
    //第一行内容  通常作为通知栏标题
    builder.setContentTitle("标题");
    //第二行内容 通常是通知正文
    builder.setContentText("通知内容");
    //第三行内容 通常是内容摘要什么的 在低版本机器上不一定显示
    builder.setSubText("这里显示的是通知第三行内容！");
    //ContentInfo 在通知的右侧 时间的下面 用来展示一些其他信息
    //builder.setContentInfo("2");
    //number设计用来显示同种通知的数量和ContentInfo的位置一样，如果设置了ContentInfo则number会被隐藏
    builder.setNumber(2);
    //可以点击通知栏的删除按钮删除
    builder.setAutoCancel(true);
    //系统状态栏显示的小图标
    builder.setSmallIcon(R.mipmap.ic_launcher);
    //下拉显示的大图标
    builder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.contact));
    Intent intent = new Intent(this, NotificationActivity.class);
    PendingIntent pIntent = PendingIntent.getActivity(this, 1, intent, 0);
    //点击跳转的intent
    builder.setContentIntent(pIntent);
    //通知默认的声音 震动 呼吸灯
    builder.setDefaults(NotificationCompat.DEFAULT_ALL);
    Notification notification = builder.build();
    manager.notify(TYPE_NORMAL, notification);
    
### 二、下载进度的通知
![下载进度的通知](https://github.com/ZLOVE320483/Notification/blob/master/img/progress.png)
>
    NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
    NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
    builder.setSmallIcon(R.mipmap.ic_launcher);
    builder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.contact));
    //禁止用户点击删除按钮删除
    builder.setAutoCancel(false);
    //禁止滑动删除
    builder.setOngoing(true);
    //取消右上角的时间显示
    builder.setShowWhen(false);
    builder.setContentTitle("下载中...7%");
    builder.setProgress(100, 7, false);
    //builder.setContentInfo(progress+"%");
    builder.setOngoing(true);
    builder.setShowWhen(false);
    Intent intent = new Intent(this, NotificationActivity.class);
    intent.putExtra("command", 1);
    PendingIntent pIntent = PendingIntent.getActivity(this, 1, intent, 0);
    //点击跳转的intent
    builder.setContentIntent(pIntent);
    builder.setDefaults(NotificationCompat.DEFAULT_ALL);
    Notification notification = builder.build();
    manager.notify(TYPE_PROGRESS, notification);
    
### 三、BigTextStyle通知
![BigTextStyle通知](https://github.com/ZLOVE320483/Notification/blob/master/img/bitText.png)
>
    NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
    NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
    builder.setContentTitle("BigTextStyle");
    builder.setContentText("BigTextStyle演示示例");
    builder.setSmallIcon(R.mipmap.ic_launcher);
    builder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.contact));
    android.support.v4.app.NotificationCompat.BigTextStyle style = new android.support.v4.app.NotificationCompat.BigTextStyle();
    style.bigText("这里是点击通知后要显示的正文，可以换行可以显示很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长很长");
    style.setBigContentTitle("点击后的标题");
    //SummaryText没什么用 可以不设置
    style.setSummaryText("末尾只一行的文字内容");
    builder.setStyle(style);
    builder.setAutoCancel(true);
    Intent intent = new Intent(this, NotificationActivity.class);
    PendingIntent pIntent = PendingIntent.getActivity(this, 1, intent, 0);
    builder.setContentIntent(pIntent);
    builder.setDefaults(NotificationCompat.DEFAULT_ALL);
    Notification notification = builder.build();
    manager.notify(TYPE_BIG_TEXT, notification);
    
### 四、InboxStyle通知
![InboxStyle通知](https://github.com/ZLOVE320483/Notification/blob/master/img/inbox.png)
>
    NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
    NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
    builder.setContentTitle("InboxStyle");
    builder.setContentText("InboxStyle演示示例");
    builder.setSmallIcon(R.mipmap.ic_launcher);
    builder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.contact));
    android.support.v4.app.NotificationCompat.InboxStyle style = new android.support.v4.app.NotificationCompat.InboxStyle();
    style.setBigContentTitle("BigContentTitle")
            .addLine("第一行，第一行，第一行，第一行，第一行，第一行，第一行")
            .addLine("第二行")
            .addLine("第三行")
            .addLine("第四行")
            .addLine("第五行")
            .setSummaryText("SummaryText");
    builder.setStyle(style);
    builder.setAutoCancel(true);
    Intent intent = new Intent(this, NotificationActivity.class);
    PendingIntent pIntent = PendingIntent.getActivity(this, 1, intent, 0);
    builder.setContentIntent(pIntent);
    builder.setDefaults(NotificationCompat.DEFAULT_ALL);
    Notification notification = builder.build();
    manager.notify(TYPE_INBOX, notification);
    
### 五、BigPictureStyle通知
![BigPictureStyle通知](https://github.com/ZLOVE320483/Notification/blob/master/img/bigPicture.png)
>
    NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
    NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
    builder.setContentTitle("BigPictureStyle");
    builder.setContentText("BigPicture演示示例");
    builder.setSmallIcon(R.mipmap.ic_launcher);
    builder.setDefaults(NotificationCompat.DEFAULT_ALL);
    builder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.contact));
    android.support.v4.app.NotificationCompat.BigPictureStyle style = new android.support.v4.app.NotificationCompat.BigPictureStyle();
    style.setBigContentTitle("BigContentTitle");
    style.setSummaryText("SummaryText");
    style.bigPicture(BitmapFactory.decodeResource(getResources(), R.mipmap.img_power_popup_notice));
    builder.setStyle(style);
    builder.setAutoCancel(true);
    Intent intent = new Intent(this, NotificationActivity.class);
    PendingIntent pIntent = PendingIntent.getActivity(this, 1, intent, 0);
    //设置点击大图后跳转
    builder.setContentIntent(pIntent);
    Notification notification = builder.build();
    manager.notify(TYPE_BIG_PICTURE, notification);
    
### 六、hangup横幅通知
![hangup横幅通知](https://github.com/ZLOVE320483/Notification/blob/master/img/hungUp.png)
>
    NotificationManager manager = (NotificationManager) getSystemService(NOTIFICATION_SERVICE);
    NotificationCompat.Builder builder = new NotificationCompat.Builder(this);
    builder.setContentTitle("横幅通知");
    builder.setContentText("请在设置通知管理中开启消息横幅提醒权限");
    builder.setDefaults(NotificationCompat.DEFAULT_ALL);
    builder.setSmallIcon(R.mipmap.ic_launcher);
    builder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.contact));
    Intent intent = new Intent(this, NotificationActivity.class);
    PendingIntent pIntent = PendingIntent.getActivity(this, 1, intent, 0);
    builder.setContentIntent(pIntent);
    //这句是重点
    builder.setFullScreenIntent(pIntent, true);
    builder.setAutoCancel(true);
    Notification notification = builder.build();
    manager.notify(TYPE_HANGUP, notification);

## 常见的几种notification样式已经介绍完毕，下面说一说我踩过的坑（前方高能，请注意）
### 坑一、notification全展示
    以上的例子中，连续发送多条notification，你会发现所有的notification都只展示了最新的一条，之前发送的都会被最后一条顶替。假如你想要展示所有的
    notification，应该怎么处理呢？只要将 
``` java
manager.notify(TYPE_HANGUP, notification);
```
    这个方法中的第一个参数id，改成不同值，就可以让不同的notification全展示了。
### 坑二、hangup横幅通知不消失
    当你发送横幅通知的时候你会发现横幅没有自动收起，我采用了下面的方法达到了自动收起的效果
``` java
new Handler().postDelayed(new Runnable() {
  @Override
  public void run() {
      manager.cancel(TYPE_HANGUP);
      NotificationCompat.Builder builder = new NotificationCompat.Builder(MainActivity.this);
      builder.setContentTitle("横幅通知");
      builder.setContentText("请在设置通知管理中开启消息横幅提醒权限");
      builder.setSmallIcon(R.mipmap.ic_launcher);
      builder.setLargeIcon(BitmapFactory.decodeResource(getResources(), R.mipmap.contact));
      Intent intent = new Intent(MainActivity.this, NotificationActivity.class);
      PendingIntent pIntent = PendingIntent.getActivity(MainActivity.this, 1, intent, 0);
      builder.setContentIntent(pIntent);
      builder.setAutoCancel(true);
      Notification notification = builder.build();
      manager.notify(TYPE_HANGUP, notification);
  }
}, 2000);
```
    延迟两秒去cancel这个notification，但是又会发现通知栏里又没有了这个notification，此时再悄无声息的再发送一次，完美解决。
### 坑三、hangup横幅通知自动唤起app
    这个坑很蛋疼，当你调用了这个方法 builder.setFullScreenIntent(pIntent, true); 并且你的app没有退出的时候，你会发现在某些手机上，你的app会自动
    跳转到你notification里设置的那个页面，即使在息屏的状态下也会。修复的办法只要将上面的代码改为 builder.setFullScreenIntent(null, true); 即可。
### 坑四、点开notification，发现intent所携带的参数丢失
    当你发送多条notification，并且每条notification所携带的参数（即想要打开的指定的页面）不同，你会发现intent里携带的参数会被最新的一条notification
    里带的参数替换，导致总是打开同一个页面。修正的方法是将 PendingIntent pIntent = PendingIntent.getActivity(MainActivity.this, 1, intent, 0);
    这句代码里的第二个参数 requestCode 设置成不同的值，即可将不同notification intent所携带的参数存住。
## end
  That's all, I hope this will help you!
