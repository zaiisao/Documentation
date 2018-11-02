
---
title: 发布和订阅音频流
description: 
platform: 微信小程序
updatedAt: Thu Nov 01 2018 08:22:47 GMT+0000 (UTC)
---
# 发布和订阅音频流
# 发布和订阅音频流

## 发布本地音频流
加入频道后，使用 发布本地音频流 `publish` 方法发布本地音频流。

```
client.publish(onSuccess, onFailure);
```

## 订阅远端音频流
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
      client.join(undefined, channel, uid, () => {
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
