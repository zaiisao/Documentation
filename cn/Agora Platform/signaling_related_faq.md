
---
title: 信令相关
description: 
platform: All Platforms
updatedAt: Mon Oct 21 2019 08:03:32 GMT+0800 (CST)
---
# 信令相关
### 怎么获取用户在线列表？

Agora 不提供获取用户在线列表的方法。 用户管理由信令处理，该功能可在信令层中实现。 信令SDK 的 `onUserJoined` 和 `onUserOffline` 回调将会在用户上线或离线频道时发出通知。

### 怎么设置 PSTN 的主叫号码？

* 如果你使用 channelInvitePhone 2，最后的参数为主叫号码。
* 如果你使用 channelInvitePhone，用户账号为主叫号码。

### Agora.io 提供 PSTN 方案吗？

Agora 不提供完整的 PSTN 解决方案，但提供从 VoIP 到 PSTN 的对接功能。

### App 的用户之间要建立和发起一个呼叫，整个流程是怎样的？

以 A 呼叫 B 为例，一般呼叫流程如下:

1. A 向信令服务器发起呼叫请求。

2. 信令服务器检查 B 是否在线：
    * 如不在线，向 A 返回 B 不在线错误。
    * 如在线，信令服务器生成频道名，返回给 A；并向 B 投递呼叫信令。

3. A 收到信令服务器返回的频道名，准备加入语音频道。此时为加快进频道速度，可以提前进入频道待命：
    * A 调用 `muteLocalAudioStream(true)` 和 `muteLocalVideoStream(true)`（如有视频功能）禁止发送音视频数据。
    * 调用` joinChannel` 进入频道。

4. B 收到信令服务器投递过来的A的呼叫请求。
   * B 响铃。为加快进频道速度，可以提交进入频道待命。
   * B 调用 `muteLocalAudioStream(true)` 和 `muteLocalVideoStream(true)`（如有视频功能）禁止发送音视频数据。

5. A 调用 `joinChannel` 进入频道：
     - 如 B 拒绝请求：
    * B 调用 `leaveChannel` 退出频道
    * B 向信令服务器返回拒绝应答
    * 信令服务器向 A 返回 B 拒绝应答信令
    * A 调用 `leaveChannel` 退出频道
     - 如 B 接受请求：
    * B 调用 `muteLocalAudioStream(false)` 和 `muteLocalVideoStream(false)` 开始发送音视频数据
    * B 向信令服务器返回接受应答信令
    * 调用 `muteLocalAudioStream(false)` 和 `muteLocalVideoStream(false)` 开始发送音视频数据

### 消息通常可以储存多久?

<table>
  <tr>
    <th>消息</th>
    <th>储存时长</th>
  </tr>
  <tr>
    <td>频道消息</td>
    <td>频道消息会确保发送</td>
  </tr>
  <tr>
    <td>一对一消息</td>
    <td>该消息会保存一天</td>
  </tr>
  <tr>
    <td>呼叫消息</td>
    <td><li>如果用户 A 呼叫 B 超过 30 秒, B 还没有收到呼叫邀请消息，该呼叫会自动挂断。</li><li>如果用户 A 呼叫 B 超过 20 秒(该值可配)， B 收到呼叫邀请后没有返回任何确认信息，A 将收到呼叫失败的通知。之后不再会收到超时通知</li></td>
  </tr>
</table>

### 信令登录失败

1. 检查网络是否正常。
2. 检查 App ID, App Certificate, 和 Signaling Key 是否正确。

### 返回 LOGIN_E_TOKENWRONG(206) 错误

该错误是由于使用的 Signaling Key 无效引起的，主要原因如下：

* 请检查 App ID 是否正确。
* 请检查 App Certificate 是否正确，是否在控制台已启用。App Certificate 在控制台上启用 1 小时后，方能生效。
* 请检查生成 token (Signaling Key) 的算法是否正确。

关于如何获取正确的 App ID 和 App Certificate, 以及如何使用正确的算法生成 Signaling Key, 详见 [校验用户权限](../../cn/Agora%20Platform/key_signaling.md)。

### 无法拨通一个已经通过验证的 PSTN 电话号码

确认您在号码前添加了国家代码。如果是固定电话或者 800 电话号码，必须在号码前加上区号。

### 打不通电话，如何判断是 APP 逻辑问题，还是 Agora.io 提供的 SDK 有问题？

建议您在 [Agora.io](https://www.agora.io/cn/)  提供的 demo 程序里测试一下，如果在 demo 程序里可以打通电话，那就是 APP 的逻辑问题。

登录不成功，没有 loginsuccess 回调？

通过 Error Code 进行初步判断：

Error 206

token 错误

* 确认 token 参数，如果是 no need token，确认 dashboard 里 token 调试开关已经打开。

![](https://web-cdn.agora.io/docs-files/1540453296247)

* 确认 app certificate 是开启的。
* 确认时间戳是 10 位，并且没有过期。
* token 过期当时不会被踢出频道，过期时间尝试登录会失败。

Error 201

* App ID 没填或 token 直接传 null 或者空，或者配置文件内的格式不对（没回车换行，有空格等等）导致没识别到 App ID。
* 网络有问题，看下是否上网正常。
* 获取 SDK 日志。

### 掉线问题

引起掉线的原因：

* 弱网或断网。
* 多个设备登录同一个 App ID 下的同一个 account ID，会踢掉上一个登录的人。
* SDK 无法连接服务器
   * Native SDK 网络不好的情况下内部会尝试重连，一旦 logout 回调出来就不再重连了。
   * Web SDK 1.4 版本的刚刚加入 SDK 的内部重连，之前版本，网络一断立即 logout。
   * Java SDK 目前没有做 SDK 内部重连。

> Web 和 Java SDK 被踢错误码和网络掉线错误码用的都是 102，Agora 之后会修复这个问题。

