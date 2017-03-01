---
layout:     post  
title:      "安卓应用角标那些事儿"  
subtitle:   "聊一聊安卓应用角标那些事儿"  
date:       2017-02-28 23:43  
author:     "wzystal"  
header-img: "img/post-bg-android-logo.jpg"  
tags:   
- Android
---

## 什么是应用角标？
&emsp;&emsp;  应用角标最开始是在ios系统中出现的，大概长这样：
&emsp; ![角标.jpg](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/f5dcc2ce5a984d7ad62269e25ff51e42.jpg)

&emsp;&emsp;  不知道从什么时候开始，国内各大安卓手机系统上，也慢慢出现了应用角标的身影，到现在几乎成为了安卓系统的标配，发张图片让大家近距离感受一下：
![screenshot.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/8bcf1bf26d4409ba9e9a36a0f4d9a832.png)

&emsp; &emsp; 但是有一点要特别提一下，那就是`原生的Android系统，是不支持应用角标的`（这也是文章标题没有叫“安卓角标”而不是“Android角标”的原因）。毕竟应用角标是一个带有争议的设计，有些强迫症用户对应用角标咬牙切齿，有些用户看不到应用角标又浑身痒痒，对于这一点我也只能说“仁者见仁，智者见智”吧。

## 如何支持应用角标？
&emsp; &emsp; 安卓应用的角标是由Launcher支持的，而原生的Android系统Launcher并没有提供角标支持，所以各大手机厂商只能自己定制Launcher来实现，然后提供接口给外部使用。主流的做法都是通过广播Intent的形式来提供接口，不同的手机系统所支持的广播Intent也不太一样，所以需要针对不同的手机系统来做适配（恭喜你又成功跳入一个系统适配的坑！）。

## 角标开源库介绍
&emsp; &emsp; 相信每个安卓开发者看到系统适配这几个字，都不会不由自主喷出一口老血。。。但是这次不用慌，因为已经有不怕死的家伙为我们造好了轮子，接下来就为大家隆重推出 [ShortcutBadger](https://github.com/leolin310148/ShortcutBadger) （github大法好啊～真是好～）。这个开源库为我们封装了各种系统适配的细节，如文档所描述的，拥有了ShortcutBadger，你也就免除了以下系统适配的烦恼：
&emsp; ![角标02.png](http://ata2-img.cn-hangzhou.img-pub.aliyun-inc.com/989eca40f7a45f24740a613796610260.png)  
&emsp; &emsp; 但在实际使用时，你会发现，图片很美，现实却很残酷！经不完全测试（原谅我的“不完全”，毕竟安卓机型碎片化太过严重），我发现这个开源库对于小米、华为、vivo和oppo等主流机型的支持都是有各种问题的，要么完全不奏效，要么就有bug。当然这个开源库也在不断完善，不过在它还不够稳定的情况下，建议大家还是先用来当备选方案，支持一些冷门机型可以，主流机型还是自己适配比较靠谱点。

## 特殊机型适配

### 华为系列
&emsp;&emsp; 传送门：[华为角标官方文档](http://developer.huawei.com/cn/consumer/wiki/index.php?title=%E5%8D%8E%E4%B8%BA%E6%A1%8C%E9%9D%A2%E8%A7%92%E6%A0%87%E4%BB%8B%E7%BB%8D)   
&emsp;&emsp; 按照华为官方文档的介绍，如果要获得应用角标支持，需要为应用申请华为证书，然后在“权签平台”下载开发者证书，再将开发者证书集成到应用中，整个流程跑下来还是很繁琐的。  
&emsp;&emsp; 那么吃瓜群众肯定就要问了，有没有简单一点的方法呢？答案当然是有的！结合华为的《角标开发指导书》中的代码参考和亲身实践，我发现只需要其中两步，也可以实现华为角标支持（目前只验证了华为mate8 和华为 p7，其他机型手头暂时没有，欢迎大家补充）：

1 在AndroidManifest.xml中申请华为角标权限  

```
<uses-permission android:name="com.huawei.android.launcher.permission.CHANGE_BADGE" />
```

2 参考以下代码，设置应用角标即可  

```
Bundle bundle = new Bundle();  
bundle.putString("package", context.getPackageName());  
String launchClassName = context.getPackageManager().getLaunchIntentForPackage(context.getPackageName()).getComponent().getClassName();  
bundle.putString("class", launchClassName);  
bundle.putInt("badgenumber", msgCount); 
context.getContentResolver().call(Uri.parse("content://com.huawei.android.launcher.settings/badge/"), "change_badge", null, bundle);  

```

###小米系列
&emsp;&emsp; 传送门：[小米角标官方文档](http://dev.xiaomi.com/docs/appsmarket/technical_docs/badge/)

&emsp;&emsp; 在小米机型上，系统默认是支持应用角标的，不过支持的方式非常反人类，导致经常有用户来反馈说角标数字不对的问题。小米官方文档关于默认情况是这么描述的：
> 当app向通知栏发送了一条通知 （通知不带进度条并且用户可以删除的），那么桌面app icon角标就会显示1。此时app显示的角标数是和通知栏里app发送的通知数对应的，即向通知栏发送了多少通知就会显示多少角标。  

&emsp;&emsp; 那么吃瓜群众肯定又要问了，What are you talking about？！不要慌，我举一个例子大家就明白了，比如说千牛客户端，每收到一条未读的聊天消息，就会往通知栏发送一条通知（由于业务需要，通知id都是同一个），假如用户收到了5条未读消息，理论上来说千牛的角标数字应该显示5，但是在小米机型上你会惊奇地发现角标数字居然是1。这是因为默认情况下，应用的角标数字跟应用往通知栏发送的通知数（`注意！这个通知数是根据通知id来计算的`）相对应的，在刚刚说的场景中，`虽然千牛往通知栏发送了5条通知，但是按照通知id来统计时，对应的通知数就是1，所以角标数字也就是显示成1了`。 
 
&emsp;&emsp; 小米官方可能也意识到了，这个奇葩的设计会导致很多用户产生困惑，所以在官方文档中也提供了修改角标数的接口。不同于其他手机系统通过广播来修改角标，小米系统是通过反射机制来修改角标，关键代码参考如下：

```
Field field = notification.getClass().getDeclaredField("extraNotification");
Object extraNotification = field.get(notification);
Method method = extraNotification.getClass().getDeclaredMethod("setMessageCount", int.class);
method.invoke(extraNotification, unreadCount);
```

###OPPO系列
&emsp;&emsp; 抱歉，没有发现官方文档。  
&emsp;&emsp; 联系过oppo的开发者客服，客服MM表示：
> oppo角标提醒目前只针对内部软件还有微信、QQ开放，其他的暂时无法提供

&emsp;&emsp; 啊，多么痛的领悟～不过客服MM也及时发了一张好人卡：
> 后续应该会计划对其他应用开放的，敬请关注我们的开发平台

&emsp;&emsp; 嗯，虽然被拒绝了，但是希望还是有的，那就继续关注[oppo开发者官网地址](http://open.oppomobile.com/admin)吧～

###VIVO系列 
&emsp;&emsp; 抱歉，没有发现官方文档。  
&emsp;&emsp; 不过最近有幸在网上看到一段代码，据说能支持vivo角标展示，抱着“死马当作活马医”的心态，我尝试了一把，最后发现居然奏效了。不多说了，贴代码共赏之：

```
Intent intent = new Intent("launcher.action.CHANGE_APPLICATION_NOTIFICATION_NUM");
intent.putExtra("packageName", context.getPackageName());
String launchClassName = context.getPackageManager().getLaunchIntentForPackage(context.getPackageName()).getComponent().getClassName();
intent.putExtra("className", launchClassName);
intent.putExtra("notificationNum", msgCount);
context.sendBroadcast(intent);
```

###Sony系列
&emsp;&emsp; 参考资料：[https://developer.sony.com/2016/06/23/xperia-home-badge-api-now-publicly-available/](https://developer.sony.com/2016/06/23/xperia-home-badge-api-now-publicly-available/)  


