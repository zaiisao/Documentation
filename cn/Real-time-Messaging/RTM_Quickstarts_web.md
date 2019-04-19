
---
title: RTM 快速开始
description: 
platform: Web
updatedAt: Fri Apr 19 2019 15:02:49 GMT+0800 (CST)
---
# RTM 快速开始
## 集成 SDK

### 本地 `npm link` 安装

使用该方式需要和诸如 webpack 或 Browserify 的模块打包器配合使用。

1. 将下载的 SDK 目录放到某路径下（下面用 `$SDK_PATH` 代替该路径），在命令行中进入该目录，运行：

    ```shell
    npm install
    ```

    > **Note:** 该命令的目的是安装 `AgoraRTM.d.ts` 的类型依赖，如果不需要使用 TypeScript 定义文件，可省略。

2. 在要使用 SDK 的项目根目录（包含 `package.json` 文件）下运行：

    ```shell
    npm link $SDK_PATH
    ```

#### 注意

本地 `npm link` 安装不会在项目的 `package.json` 中写入依赖，无法在未运行上述命令的环境中使用 SDK。
该方法只作为 npm 发布前的临时解决方案，请勿在生产环境中使用。

### 直接用 `<script>` 引入

直接下载并用 `<script>` 标签引入，`AgoraRTM` 会被注册为一个全局变量。

在 HTML 文件中添加（其中 `/path/to/agora-rtm-sdk.min.js` 替换为 `agora-rtm-sdk.min.js` 所在 URL）：

```html
<script src="/path/to/agora-rtm-sdk.min.js"></script>
```

> **Note:** 确保 `src` 中的地址可在页面 URL 下访问。

#### 使用 TypeScript 定义文件（`AgoraRTM.d.ts` 文件）开启智能提示

使用 `<script>` 标签引入方式获得全局 `AgoraRTM` 对象后，可以在其余 JavaScript 代码中使用，但无法开启智能提示。
要开启智能提示，需要在 JavaScript 文件开头加入下面的注释（其中 `path/to/AgoraRTM.d.ts` 替换为 `AgoraRTM.d.ts` 所在路径）：

```JavaScript
/// <reference path="path/to/AgoraRTM.d.ts" />
```

## 初始化 SDK 客户端

### 创建实例

登入 RTM 之前，调用 `AgoraRTM.createInstance` 方法创建一个 `RtmClient` 实例。

创建实例需要填⼊准备好的 App ID, 只有 App ID 相同的应⽤才能互通。

```JavaScript
import AgoraRTM from 'agora-rtm-sdk';
const client = AgoraRTM.createInstance('demoAppId'); // 传入 App ID
```

### 传入 Token 与初始化（可选）

传入能标识用户角色和权限的 Token。如果安全要求不高，也可以跳过本步骤。Token 需要在应用程序的服务器端生成。

```JavaScript
client.init({ token: 'demoToken' }) // token 为从服务端获取的 Token
```

## 点对点消息

App 在成功登录 RTM 服务器之后，可以开始使用 RTM 的点对点消息功能。

### 发送点对点消息

调用 `client` 上的 `sendMessageToPeer` 方法发送点对点消息。在该方法中：

传入目标消息接收方的用户账号 ID。
传入符合 `RtmMessage` 接口的参数对象。
该方法返回一个 Promise：

该 Promise 执行（resolve）的值指示消息接收方是否已收到该消息。
该 Promise 拒绝（reject）的情况可能为超时、服务器拒绝或连接失败等。

```JavaScript
client.sendMessageToPeer(
  'demoPeerId', // 远端用户 ID
  { text: 'test peer message' }, // 符合 RtmMessage 接口的参数对象
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
client.on('MessageFromPeer', { text }, peerId) => { // text 为消息文本，peerId 是消息发送方 User ID
  /* 收到点对点消息的处理逻辑 */
});
```

## 频道消息

### 创建并加入频道

创建频道：

```JavaScript
const channel = client.createChannel('demoChannelName'); // 此处传入频道 ID
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

```JavaScript
channel.on('ChannelMessage', ({ text }, senderId) => { // text 为收到的频道消息文本，senderId 为发送方的 User ID
  /* 收到频道消息的处理逻辑 */
});
```

### 监听频道中用户加入/离开通知

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

