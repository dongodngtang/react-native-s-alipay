
# react-native-s-alipay
React Native 支付宝模块，同时支持ios和android，react native 0.60.0+  autolink

## 安装

```js
npm install react-native-s-alipay --save
```
或者

```js
yarn add react-native-s-alipay
```

## 配置
如果你的react native >= 0.60.0，那么你不需要做太多的配置。

## Android:
android需要在android -> app -> build.gradle中配置lib包的引用：

```js
...
repositories {
    flatDir {
        dirs project(':react-native-s-alipay').file('libs')
    }
}
...
```

## ios:

## 第一步：

ios需要在ios项目的Podfile中加入支付宝依赖包的引用：
```js
pod 'AlipaySDK-iOS'
```
然后运行命令：

```js
cd ios && pod install
```
## 第二步：
等待安装成功后，进入ios工程文件夹，会看到一个.xcworkspace 结尾的文件 ，双击打开
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191230213920305.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaGFwcHlfbG9uZw==,size_16,color_FFFFFF,t_70)

## 第三步：
选中项目，右键添加文件
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191230214044971.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaGFwcHlfbG9uZw==,size_16,color_FFFFFF,t_70)
点击找到本项目node_modules下的react-native-s-alipay -> iosLib -> RNSAlipay, 将整个RNSAlipay文件夹导入。

## 第四步
在项目中创建bridging header文件，选中项目，右键New File，选择Header File，命名为 你的工程名 + -Bridging-Header，如：example-Bridging-Header，然后在该文件夹中加入以下代码：

```js
#import <React/RCTBridgeModule.h>
#import <AlipaySDK/AlipaySDK.h>
```

## 第五步
配置桥接文件：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191230220214202.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaGFwcHlfbG9uZw==,size_16,color_FFFFFF,t_70)

## 第六步：
在AppDelegate.m文件中添加以下两个方法，来处理跳转的url：

头部引用：

```js
#import <AlipaySDK/AlipaySDK.h>
```

```js
- (BOOL)application:(UIApplication *)application
            openURL:(NSURL *)url
  sourceApplication:(NSString *)sourceApplication
         annotation:(id)annotation {
    
    if ([url.host isEqualToString:@"safepay"]) {
        //跳转支付宝钱包进行支付，处理支付结果
        [[AlipaySDK defaultService] processOrderWithPaymentResult:url standbyCallback:^(NSDictionary *resultDic) {
            NSLog(@"result = %@",resultDic);
        }];
    }
    return YES;
}

// NOTE: 9.0以后使用新API接口
- (BOOL)application:(UIApplication *)app openURL:(NSURL *)url options:(NSDictionary<NSString*, id> *)options
{
    if ([url.host isEqualToString:@"safepay"]) {
        //跳转支付宝钱包进行支付，处理支付结果
        [[AlipaySDK defaultService] processOrderWithPaymentResult:url standbyCallback:^(NSDictionary *resultDic) {
            NSLog(@"result = %@",resultDic);
        }];
    }
    return YES;
}
```

## 第七步：
添加url type：
![在这里插入图片描述](https://img-blog.csdnimg.cn/20191230215350413.jpg?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3VuaGFwcHlfbG9uZw==,size_16,color_FFFFFF,t_70)
到此支付宝配置结束。

## 使用
```javascript
import Alipay from 'react-native-s-alipay';

//orderInfo由后端返回
Alipay.pay(orderInfo).then(
  res => {
    console.log('success:', res);
  },
  res => {
    console.log('fail:', res);
  },
);
```
