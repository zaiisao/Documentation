
---
title: 设置日志信息
description: 
platform: Web
updatedAt: Wed Feb 20 2019 07:05:09 GMT+0000 (UTC)
---
# 设置日志信息
## 功能描述
Agora Web SDK 提供设置 logger 的方法，包括设置日志输出等级、开启/关闭日志上传。

## 实现方法
开始前请确保你已完成环境准备、安装包获取等步骤，详见[集成客户端 ](../../cn/Video/web_prepare.md)。

### 设置日志输出等级
日志输出等级依次为 NONE，ERROR，WARNING，INFO，DEBUG。选择一个级别，你就可以看到在该级别及之前所有级别的日志信息。例如，输入代码 `AgoraRTC.Logger.setLogLevel(AgoraRTC.Logger.INFO);`，就可以看到 WARNING、ERROR 和 INFO 级别的日志信息。

#### API 参考

[`setLogLevel`](https://docs.agora.io/cn/Video/API%20Reference/web/modules/agorartc.logger.html#setloglevel)

### 开启/关闭日志上传
Agora Web SDK 通过 `enableLogUpload` 方法将 SDK 的日志上传到 Agora 的服务器，通过 `disableLogUpload` 方法停止上传。

#### 示例代码
`AgoraRTC.Logger.enableLogUpload();`

`AgoraRTC.Logger.disableLogUpload();`

#### 开发注意事项
- 日志上传功能默认为关闭状态，如果你需要开启此功能，请确保在所有方法之前调用 `enableLogUpload` 方法。
- 如果没有成功加入频道，服务器上无法查看日志信息。

#### API 参考

- [`enableLogUpload`](https://docs.agora.io/cn/Video/API%20Reference/web/modules/agorartc.logger.html#enablelogupload)
- [`disableLogUpload`](https://docs.agora.io/cn/Video/API%20Reference/web/modules/agorartc.logger.html#disablelogupload)
