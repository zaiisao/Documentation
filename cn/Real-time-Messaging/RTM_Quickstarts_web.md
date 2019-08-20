
---
title: RTM 快速开始
description: 
platform: Web
updatedAt: Tue Aug 20 2019 07:28:30 GMT+0800 (CST)
---
# RTM 快速开始

## 集成客户端

### 直接用 `<script>` 引入

使用该方法引入的 SDK 会在 `window` 上生成名为 `AgoraRTM` 的全局变量。

1. 从 Agora 官方网站下载最新版 [Agora RTM SDK for Web](../../cn/Real-time-Messaging/downloads.md) 压缩包。
2. 将下载下来的压缩包中路径为 `libs/agora-rtm-sdk-1.0.0.js` 的文件保存到你所操作的项目下。
3. 在项目相应的前端页面文件中，对刚才保存的 SDK 文件进行引用（其中 `/path/to/agora-rtm-sdk-1.0.0.js` 替换为可访问的 SDK 公开网址）：

    ```html
    <script src="/path/to/agora-rtm-sdk-1.0.0.js"></script>
    ```

#### 开启智能提示和类型检查（可选）

使用 `<script>` 标签引入后，可以在 JavaScript/TypeScript 代码中使用全局 `AgoraRTM` 变量，但无法开启智能提示和类型检查。

可以通过下面的步骤开启上述功能：

1. 将压缩包中路径为 `libs/agora-rtm-sdk.d.ts` 的文件保存到你所操作的项目下。
2. 在 JavaScript/TypeScript 文件开头加入下面的注释（其中 `path/to/agora-rtm-sdk.d.ts` 替换为 `agora-rtm-sdk.d.ts` 所在本地路径）：

```JavaScript
/// <reference path="path/to/agora-rtm-sdk.d.ts" />
```

## 初始化

登入 RTM 之前，调用 `AgoraRTM.createInstance` 方法创建一个 `RtmClient` 实例。

创建实例需要填⼊准备好的 App ID, 只有 App ID 相同的应⽤才能互通。
> 示例代码中的 `client` 变量为 RtmClient 实例，下同。

```JavaScript
const client = AgoraRTM.createInstance('<APPID>');
```

### 监听连接状态改变

通过监听 `RtmClient` 上的 `ConnectionStateChanged` 事件可以获得 SDK 连接状态改变的通知。

```JavaScript
client.on('ConnectionStateChanged', (newState, reason) => {
  console.log('on connection state changed to ' + newState + ' reason: ' + reason);
});
```

## 登录

Web 应用必须在登录 RTM 服务器之后，才可以使用 RTM 的点对点消息和群聊功能。

在 `client.login` 方法中传入一个有如下属性的对象：

* `token`: 如果安全要求不高，不填或将参数值设为 null；如果安全要求高，传入从你的服务端获得的 token 值。
* `uid`: User ID 为字符串，必须是可见字符（可以带空格），不能为空或者多于64个字符，也不能是字符串 “null”。

该方法的返回值为 Promise。使用该 Promise 上的 `then` 方法传入回调；使用 `catch` 方法捕获异常并处理。也可以使用 ES7 的 `async/await` 语法来进行 SDK 异步方法的调用，这样就可以使用同步 `try/catch` 块来捕获异常（其他返回 Promise 的异步 API 也均如此）。

> 示例代码中均使用 then/catch 方法传入回调与捕获异常。

```JavaScript
client.login({ token: '<TOKEN>', uid: '<UID>' }).then(() => {
  console.log('AgoraRTM client login success');
}).catch(err => {
  console.log('AgoraRTM client login failure', err);
});
```

如果需要退出登录，可以调用 logout 方法，退出登录之后可以调用 login 重新登录或者切换账号。

```JavaScript
client.logout();
```

## 点对点消息

App 在成功登录 RTM 服务器之后，可以开始使用 RTM 的点对点消息功能。

### 发送点对点消息

调用 `client` 上的 `sendMessageToPeer` 方法发送点对点消息。在该方法中：

* 传入目标消息接收方的用户账号 ID。
* 传入符合 `RtmMessage` 接口的参数对象。

该方法返回一个 Promise：  
该 Promise 执行（resolve）的值指示消息接收方是否已收到该消息。  
该 Promise 拒绝（reject）的情况可能为超时、服务器拒绝或连接失败等。

```JavaScript
client.sendMessageToPeer(
	{ text: 'test peer message' }, // 符合 RtmMessage 接口的参数对象
	'<PEER_ID>', // 远端用户 ID
).then(sendResult => {
  if (sendResult.hasPeerReceived) {
    /* 远端用户收到消息的处理逻辑 */
  } else {
    /* 服务器已接收、但远端用户不可达的处理逻辑 */
  }
}).catch(error => {
  /* 发送失败的处理逻辑 */
});
```

### 接收点对点消息

监听 `client` 上的事件 `MessageFromPeer` 接收点对点消息。

```JavaScript
client.on('MessageFromPeer', ({ text }, peerId) => { // text 为消息文本，peerId 是消息发送方 User ID
  /* 收到点对点消息的处理逻辑 */
});
```

## 频道消息

### 创建并加入频道

创建频道：

```JavaScript
const channel = client.createChannel('<CHANNEL_ID>'); // 此处传入频道 ID
```

加入频道（channel 为刚才创建的频道实例，每个频道 ID 都需要创建一个独立的频道实例，下同）：

```JavaScript
channel.join().then(() => {
  /* 加入频道成功的处理逻辑 */
}).catch(error => {
  /* 加入频道失败的处理逻辑 */
});
```

### 发送频道消息

加入频道成功后可发送频道消息。

```JavaScript
channel.sendMessage({ text: 'test channel message' }).then(() => {
  /* 频道消息发送成功的处理逻辑 */
}).catch(error => {
  /* 频道消息发送失败的处理逻辑 */
});
```

### 接收频道消息

加入频道后可通过监听 `RtmChannel` 实例上的 `ChannelMessage` 事件接收到频道消息。

```JavaScript
channel.on('ChannelMessage', ({ text }, senderId) => { // text 为收到的频道消息文本，senderId 为发送方的 User ID
  /* 收到频道消息的处理逻辑 */
});
```

### 监听频道成员加入/离开通知

加入频道后可通过监听 `RtmChannel` 实例上的 `MemberJoined`/`MemberLeft` 事件获得频道成员加入/离开通知。

```JavaScript
channel.on('MemberJoined', memberId => { // memberId 为加入频道的用户 ID
  /* 收到频道用户加入通知的处理逻辑 */
})
```

```JavaScript
channel.on('MemberLeft', memberId => { // memberId 为离开频道的用户 ID
  /* 收到频道用户离开通知的处理逻辑 */
})
```

### 获取频道成员列表

加入频道后可获取频道成员列表。调用实例的 `getMembers` 方法可以获取到当前在该频道内的用户列表。

```JavaScript
channel.getMembers().then(membersList => { // membersList 为获取到的频道成员列表
  /* 获取频道成员列表成功的处理逻辑 */
}).catch(error => {
  /* 频道消息发送失败的处理逻辑 */
});
```

### 退出频道

调用实例的 leave 方法可以退出该频道。退出频道之后可以调用 join 方法再重新加入频道。

```JavaScript
channel.leave();
```

