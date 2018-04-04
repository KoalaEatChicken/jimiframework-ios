# jimiframework-ios
sdk对应的服务端接入文档，请移步：  [考拉游戏平台sdk服务端接入文档](./docs/考拉游戏平台sdk服务端接入文档%20v1.0.md)


# 考拉游戏平台SDK【iOS端】接入文档 v1.x



## 1. 接入准备

1）获取sdk文件，包括`JIMI.bundle`，`JIMI.framework`，`JIMIParam.plist`。

*  JIMI.bundle为sdk资源文件；
* JIMI.framework为sdk逻辑实现文件；
*  JIMIParam.plist为游戏参数，请对应后台提供的参数(appid、appkey、ver、version等)进行修改。 

2）工程的info.plist文件添加以下权限

```xml
<key>NSAppTransportSecurity</key>
<dict>
    <key>NSAllowsArbitraryLoads</key>
    <true/>
</dict>
```

`Privacy - Photo Library Usage Description`

`Privacy - Photo Library Additions Usage Description`

3）配置参数文件`JIMIParam.plist`

![plist_tip][plist_tip]

+ 参数说明

  > version字段说明：
  >
  > 1、相同ver情况下，保证唯一性的一个字符串(不能包含小数点)；
  >
  > 2、建议生成规则：游戏版本号去掉小数点（出包前修改后的版本号）；
  >
  > eg.游戏版本：7.5.0，则version可填750


## 2. 快速接入

### 2.1 注册通知

* 请在对应的位置注册，并且实现对应方法

####2.1.1 退出按钮

```objective-c
//请在适当位置注册点击浮点里退出按钮的通知
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(switchuser:) name:@"JavascriptSwitchUser" object:nil];

// 收到退出的通知
- (void)switchuser:(NSNotification *)sender
{
    //此处做点击退出
    NSLog(@"这个是点击浮点里退出按钮的回调");
}
```

#### 2.1.2 充值的通知

```objective-c
//第一步，在AppDelegate.m文件中作如下添加：
#import <JIMI/JIMIFile.h>
- (void)applicationWillEnterForeground:(UIApplication *)application
{
    [[JIMIFile JIMIShare] gameWillEnterForeground];
}
```

```objective-c
//第二步，请在适当位置注册从充值跳转回来的通知
[[NSNotificationCenter defaultCenter] addObserver:self selector:@selector(updateData:) name:@"kaolaZF" object:nil];

- (void)updateData:(NSNotification *)sender
{
    //sdk充值后将返回此方法
    NSLog(@"这个是sdk充值后的回调");
}
```

### 2.2 初始化

```objective-c
[[JIMIFile JIMIShare] gameInitSuccess:^(NSString *paramer) {
    NSLog(@"gameInitSuccess:返回是否测试提审 1测试 %@",paramer);
} andFailure:^{
	NSLog(@"初始化失败");
}];
```

### 2.3 登录

```objective-c
//登录接口，在初始化完成后即可调用
[[JIMIFile JIMIShare] gameLoginSuccess:^(NSDictionary *dict) {
    NSLog(@"登录成功:%@",dict);
} andFailure:^{

}];
```

### 2.4 充值

```objective-c
[[JIMIFile JIMIShare] gameIAPWithBillno:@"订单号" flag:@"对应苹果后台的商品标示" amount:@"金额（单位元）” serverid:@"服务器id" rolename:@"角色名称" rolelevel:@"角色等级" subject:@"商品名称" extrainfo:@"额外信息"];
```

### 2.5 登出

```objective-c
[[JIMIFile JIMIShare] gameLoginoutSuccess:^{
    NSLog(@“登出成功，需要重新初始化");
} andFailure:^{

}];
```



## 3. 常见问题FAQ









[plist_tip]:./docs/doc_res/plist_tip.png	"配置参数"

