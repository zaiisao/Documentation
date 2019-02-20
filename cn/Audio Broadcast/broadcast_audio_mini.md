
---
title: 实现语音直播
description: 
platform: 微信小程序
updatedAt: Mon Oct 08 2018 03:07:25 GMT+0000 (UTC)
---
# 实现语音直播
# 实现语音直播

在本页你可以了解如何使用 Agora Miniapp SDK 实现语音通话。点击 [声网小程序 Demo 体验](../../cn/Quickstart%20Guide/miniapp_demo.md) 进行 Demo 体验。

## 准备环境

1.  关于具体的开发环境要求、如何获取 App ID 以及 SDK 集成方法，详见 [设置开发环境](../../cn/Quickstart%20Guide/miniapp_video.md) 。

2.  参考 [声网小程序示例代码](https://github.com/AgoraIO/Agora-Miniapp-Tutorial) 了解如何从头创建一个示例项目。


## 使用 API 实现小程序语音直播功能

小程序 API 调用流程图如下：

<img alt="../_images/mini_flow.jpg" src="https://web-cdn.agora.io/docs-files/cn/mini_flow.jpg" style="width: 559.3px; height: 557.9px;"/>


实现小程序语音直播主要步骤为：

1.  [初始化客户端对象](#initialize)

2.  [加入频道](#join)

3.  [发布本地音频流](#publish)

4.  [订阅远端音频流](#subscribe)

5.  [离开频道](#leave)


如果你的小程序中有切后台的场景需求，还需调用如下方法做好重连机制：

-   [重新加入频道](#rejoin)


在小程序与 Native SDK 互通的场景下，如果 Native 端的频道模式为直播模式，且小程序端作为观众加入频道，则还需调用如下接口设置用户角色：

-   [设置用户角色](#role)

<a name = "initialize"></a>

### 初始化客户端对象

将项目的 App ID 填入 初始化客户端对象 `init` 方法，即可初始化客户端对象。

```
client.init(appId, onSuccess, onFailure);
```


> -   由于小程序 SDK 支持与 Native SDK 和 Web SDK 互通，请确保各 SDK 使用相同的 App ID。
> -   如果你的 App ID 启用了 App Certificate，则在 [加入频道](#join) 时必须使用 Channel Key 或 Token。

<a name = "join"></a>

### 加入频道

初始化客户端对象后，在成功的回调中调用  加入频道 `join` 方法，并在该方法中填入以下参数值：

-   channelKey：在用户服务器端生成的动态密钥（Channel Key 或 Token）。详细生成办法见 [密钥说明](../../cn/Agora%20Platform/token.md) 。如果没有开启动态密钥，设置为 `null`。

-   channel：能标识频道的频道名。

-   uid：用户的 ID，整数，需保证唯一性。如果不指定，即用户 ID 设置为 `null`，回调会返回一个服务器分配的 uid。


```
client.join(channelKey, channel, uid, onSuccess, onFailure);
```

<a name = "publish"></a>

### 发布本地音频流

加入频道后，使用 发布本地音频流 `publish` 方法发布本地音频流。

```
client.publish(onSuccess, onFailure);
```

<a name = "subscribe"></a>

### 订阅远端音频流

订阅远端音频流步骤如下：

1.  监听事件 `on`。当有人发布音频流到频道里时，会收到该事件。


```
client.on(event, callback);
```

2.  收到事件后，在回调中调用 订阅远端音频流 `subscribe`方法订阅远端音频流。


```
client.subscribe(uid, onSuccess, onFailure);
```

<a name = "leave"></a>

### 离开频道

使用 退出频道 `leave` 方法让用户离开当前直播（频道）。

```
client.leave(onSuccess, onFailure);
```

<a name = "rejoin"></a>

### 重新加入频道

重连与正常加入频道类似，即将 进入频道 `join` 方法替换为 重新加入频道 `rejoin` 方法，且该方法需要加上当前频道内所有用户的 uid 列表作为额外参数。 重连后会重新发布和订阅流，参考 Github 示例代码更新推流/拉流地址即可。该过程可以参考 Github 开源 [示例项目](https://github.com/AgoraIO/Agora-Miniapp-Tutorial) 的 `reinitAgoraClient` 方法。

```
client.rejoin(channelKey, channel, uid, uids, onSuccess, onFailure);
```

<a name = "role"></a>

### 设置用户角色

使用 设置用户角色 `setRole` 方法设置用户角色。

```
client.setRole(role);
```

> 该方法必须在 进入频道 `join` 前调用才能生效。如果用户已经在频道中，则需要先退出频道，使用该方法设置好用户角色后，再次加入频道。

**示例代码**

### 初始化客户端、加入频道、发布音频流

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

### 注册并监听流事件

```
/**
  * 有新的音频流加入频道
  */
client.on("stream-added", e => {
  let uid = e.uid;
  const ts = new Date().getTime();
  console.log(`stream ${uid} added`);
  /**
    * 订阅相应 Url 地址的音频流
    */
  client.subscribe(uid, (url, rotation) => {
    console.log(`stream ${uid} subscribed successful`);
    // 将 Url 地址发送至 live-player
  }, e => {
    console.log(`stream subscribed failed ${e} ${e.code} ${e.reason}`);
  });
});
```

### 重连

```
client.init(APPID, () => {
  // uids 为频道中已有的用户 UID 列表
  client.rejoin(undefined, channel, uid, uids, () => {
    Utils.log(`client join channel success`);
    // 获取本地推流 Url 地址
    client.publish(url => {
      Utils.log(`client publish success`);
      resolve(url);
    }, e => {
      Utils.log(`client publish failed: ${e.code} ${e.reason}`);
      reject(e)
    });
  }, e => {
    Utils.log(`client join channel failed: ${e.code} ${e.reason}`);
    reject(e)
  })
}, e => {
  Utils.log(`client init failed: ${e} ${e.code} ${e.reason}`);
  reject(e);
});
```

更多功能实现，及详细的声网小程序 API 说明及用法，请参考 [声网小程序 API 参考](https://docs.agora.io/cn/Audio%20Broadcast/API%20Reference/wechat/index.html) 。

## 实现声网 SDK 互通

声网 Agora 目前支持小程序 SDK、Native SDK 和 Web SDK 之间的互通。你可以根据实际应用场景，选择如下互通方式，并根据互通方式，选择对应的集成组合：

-   小程序 SDK 之间互通，参考 [集成小程序 SDK](../../cn/Quickstart%20Guide/miniapp_video.md)；

-   小程序 SDK 与 Native SDK 之间互通，参考 [集成小程序 SDK](../../cn/Quickstart%20Guide/miniapp_video.md) 和 [集成 Native SDK](#integrate_native)；

-   小程序 SDK 与 Web SDK 之间互通，参考 [集成小程序 SDK](../../cn/Quickstart%20Guide/miniapp_video.md) 和 [集成 Web SDK](#integrate_web)；

-   小程序 SDK 与 Native SDK 和 Web SDK 分别互通，参考 [集成小程序 SDK](../../cn/Quickstart%20Guide/miniapp_video.md)、[集成 Native SDK](#integrate_native) 和 [集成 Web SDK](#integrate_web)。


<a name = "integrate_native"></a>
### 集成 Native SDK

目前声网 Android 和 iOS 平台的 SDK 可与小程序 SDK 互通，实现语音通话。请参考下列文档进行集成。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>平台</strong></td>
<td><strong>集成方法</strong></td>
</tr>
<tr><td>Android</td>
<td><a href="../../cn/Quickstart%20Guide/broadcast_audio_android.md"><span>实现语音直播</span><a></td>
</tr>
<tr><td>iOS</td>
<td><a href="../../cn/Quickstart%20Guide/broadcast_audio_ios.md"><span>实现语音直播</span></a></td>
</tr>
</tbody>
</table>


<a name = "integrate_web"></a>
### 集成 Web SDK

根据实际功能场景需要，参考相应文档进行集成。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>平台</strong></td>
<td><strong>集成方法</strong></td>
</tr>
<tr><td>Web</td>
<td><a href="../../cn/Quickstart%20Guide/broadcast_audio_web.md"><span>入门: 实现语音直播</span></a></td>
</tr>
</tbody>
</table>


> -   当使用不同的 Agora SDK 互通时，请确保在集成各 SDK 时传入的 App ID 是一致的。
> -   在有小程序 SDK 与 Native SDK 互通的场景中，如果 Native 端的频道为直播模式，且小程序端作为观众端加入频道，则需要在 进入频道 `join` 加入频道前调用 设置用户角色 `setRole`，并将用户角色设置为观众。
> -   在有 Native SDK 与 Web SDK 互通的场景中，如果要从通信模式切换为直播模式，Native SDK 在调用 `setChannelProfile` 方法后，还需要调用 `enableWebSdkInteroperability` 以打开与 Web SDK 的互通。
> -   Web 与小程序互通时，Web 端只支持 H264 模式的编码，不支持 VP8。将 Web SDK 的 index.html 文件修改为如下设置即可：
> ```
> client = AgoraRtc.createClient({mode: 'h264_interop'})；
> ```

## 常见问题回答

### 推流/拉流处理

推流/拉流处理可以参考或直接使用 [Github 上开源代码](https://github.com/AgoraIO/Agora-Miniapp-Tutorial)。

### 退后台处理

可以通过设置小程序的 live-pusher 组件中的 waiting-image 属性来处理。设置后，推流端退到后台时，可以推送静态图片来维持推流，其他端会收到本端预设的 waiting-image 图片来代替视频流。 除非通过一些方式 \(例如后台播放背景音乐\)，小程序会在某些场景下断开 websocket 或者 rtmp 连接，例如点击右上角按钮将程序退到后台。这种情况下，若回到前台后收到 error code 904 或 501，则应使用 SDK 进行重连，具体方法请参考 重新加入频道 `rejoin` 中的描述。

### 只启用小程序的音频功能，不需要发送视频，应该如何设置？

直接使用微信小程序的接口处理即可。在小程序的 live-pusher 组件中，通过设置 “enable-camera” 来实现开启/关闭摄像头。详见 [小程序 live-pusher 组件文档](https://developers.weixin.qq.com/miniprogram/dev/component/live-pusher.html)。

### 小程序和 Native 互通有问题？

在使用小程序 SDK 过程中，请确保已在 Native 端调用如下接口完成设置，否则可能会出现无法互动，Native 端听不到小程序声音等问题：

请调用设置用户角色 `setClientRole`，并将 Role 设置为 **AgoraClientRoleBroadcaster = 1：主播**；

### 小程序和 Web 互通时，Web 端可以看到小程序的视频，但小程序看不到 Web 端的视频？

Web 与小程序互通时，Web 端只支持 H264 模式的编码，不支持 VP8。将 Web SDK 的 index.html 文件修改为如下设置即可：

```
client = AgoraRtc.createClient({mode: 'h264_interop'})；
```

### 小程序的 live-pusher 组件里的 src 应该写什么？

客户端调用 client.publish 方法后，会回调一个以 rtmp 开头的临时地址，这个地址就分别是 *live-player* 和 *live-pusher* 组件中的 **rtmp 播放地址** 和 **rtmp 推流地址**。

### 集成小程序 SDK 时，如何打开和保存日志？

调用如下 API 实现保存和打开日志：

-   保存日志：

```
AgoraMiniappSDK.LOG.onlog = (text) => {
  Utils.log(text);
};
```

-   打开日志：

```
AgoraMiniappSDK.LOG.setLogLevel(-1);
```

### 出现客户端初始化失败之后，该如何做?

1.  退出后重新加入频道；

2.  如果步骤 1 无法生效，请换台设备试试；

3.  如果步骤 2 仍旧无效，请联系客户支持。



