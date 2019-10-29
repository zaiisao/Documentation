
---
title: Native SDK 相关
description: 
platform: All Platforms
updatedAt: Tue Oct 29 2019 03:00:50 GMT+0800 (CST)
---
# Native SDK 相关
## RESTful API  认证

在使用 RESTful API 前，你需要在 HTTP 请求头部中填入 `Authorization` 字段，进行认证。本文提供 Java 和 Swift 语言生成 Authorization 字段的示例代码，请参考代码获取相应的 `Authorization` 值。

你需要在代码中填入 Customer ID 和  Customer Certificate。登录[控制台](https://dashboard.agora.io)，点击右上角账户名，进入下拉菜单 RESTful API 页面，即可获取 Customer ID 和 Customer Certificate。

> Customer ID 和 Customer Certificate 仅用于访问 RESTful API。

```java
// Java
// 填入你获取到的 Customer ID 和 Customer Certificate
String plainCredentials = "customerId:customerCertificate";
// 这里的 base64Credentials 就是你要的 Authorization 值
String base64Credentials = new String(Base64.encodeBase64(plainCredentials.getBytes()));
```

```swift
// Swift
let username = "customerId"
let password = "customerCertificate"
let loginString = String(format: "%@:%@", username, password)
let loginData = loginString.data(using: String.Encoding.utf8)!
// 这里的 base64LoginString 就是你要的 Authorization 值
let base64LoginString = loginData.base64EncodedString()
```

## 踢人API

### 功能介绍

RESTful 踢人 API：`https://api.agora.io/dev/v1/kicking-rule `，具体使用方法请参见[控制台 RESTful API](../../cn/API%20Reference/dashboard_restful_live.md)。

### 适用场景

遇到炸房等紧急场景，可以使用RESTful踢人API将捣乱者踢出，维持频道内正常秩序。

### 不适用场景

由于 RESTful 踢人 API 使用媒体后台信令，因此不建议用户大批量频繁调用，如果用户期望通过踢人 API 实现结束录制，建议客户在应用层实现。

## 两人语音聊天室集成 FAQ

### 客户场景

社交行业 LB 客户：1 v 1 音频聊天室，通信模式，客户集成代码简单，对错误回调或者质量参数缺少监控，问题发生时无法快速定位。

### 集成问题及解决方案

#### 频道内用户听对方声音小

问题原因：用户使用通信模式，通信模式默认走听筒，公放场景客户不点公放，通话音量路由不会切换到扬声器，容易造成用户听声音特别小。

解决方案：建议用户设置 `setEnableSpeakerphone`：YES，将默认路由设置为扬声器。

#### 业务部门反馈声音效果差，技术部门缺少录音验证

问题原因：客户对质量参数没有进行监控

解决方案：
* 监控 `onFirstLocalAudioFrame` 验证本地推流是否成功；
* 监控 `onFirstRemoteAudioFram` 验证远端拉留是否成功；
* 监控 `onRtcStats` 收集上行发送码率，下行接收码率，流量，客户端 CPU 使用率；
* 监控 `onAudioVolumeIndication` 收集频道内用户音量值，判断客户端采集音量是否正常；
* 监控 `onRecordFrame` 验证音频前处理阶段麦克风采集的声音是否有问题。

#### onError 中重复调用 leaveChannel 导致报错 ERR_LEAVE_CHANNEL_REJECTED = 18?

问题原因：声网一个 `joinChannel` 对应一个 `leaveChannel`，如果调用多次 `leaveChannel` 会导致 ERR_LEAVE_CHANNEL_REJECTED = 18 错误。

解决方案：检查应用逻辑，同时增加 `onError` 错误判断，如果有 ERR_LEAVE_CHANNEL_REJECTED = 18 则不再执行 `leaveChannel` 避免死循环。

