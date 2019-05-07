
---
title: 发布和订阅音视频流
description: 小程序平台发布音视频流
platform: 微信小程序
updatedAt: Thu Dec 13 2018 09:11:53 GMT+0800 (CST)
---
# 发布和订阅音视频流
在发布和订阅音视频流前，请确保你已完成环境准备、安装包获取等步骤，并成功加入频道，详见[客户端集成](../../cn/Video/miniapp_video.md)。

## 实现方法
### 发布本地音视频流
加入频道后，使用 发布本地音视频流 `publish` 方法发布本地音视频流。

```
client.publish(onSuccess, onFailure);
```


### 订阅远端音视频流
订阅远端音视频流步骤如下：

1.  监听事件 `on`。当有人发布音视频流到频道里时，会收到该事件。


```
client.on(event, callback);
```

2.  收到事件后，在回调中调用 订阅远端音视频流 `subscribe`方法订阅远端音视频流。


```
client.subscribe(uid, onSuccess, onFailure);
```

示例代码：

```
let client = new AgoraMiniappSDK.Client();
    this.client = client;
    //初始化客户端对象
    client.init(APPID, () => {
      console.log(`client init success`);
      //加入频道
      client.join(, channel, uid, () => {
        console.log(`client join channel success`);
        //注册流事件
        this.subscribeEvents(client);

        //发布本地音频流并获取推流 url 地址
        client.publish(url => {
          console.log(`client publish success`);
        }, e => {
          console.log(`client publish failed: ${e}`);
        });
      }, e => {
        console.log(`client join channel failed: ${e}`);
      })
    }, e => {
      console.log(`client init failed: ${e}`);
    });
		
```

## 相关文档
你已成功开始通话/直播。通话/直播结束后，可以使用 Agora SDK 退出当前频道：

- [离开频道](../../cn/Video/leave_mini.md)
