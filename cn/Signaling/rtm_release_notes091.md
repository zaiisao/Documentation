
---
title: Agora RTM SDK v0.9.1 版本更新信息
description: 
platform: Android
updatedAt: Fri Apr 12 2019 12:48:09 GMT+0800 (CST)
---
# Agora RTM SDK v0.9.1 版本更新信息
本文列举Agora RTM Android SDK v0.9.1 版本较 v0.9.0 版本的差异

1. 将 IResultCallback 类重命名为 ResultCallback 以遵循 Java 命名规范。
2. 删除用于监听消息发送状态的 IStateListener 类，代以 ResultCallback 类监听点对点消息和频道消息的发送结果。
   - 成功: SDK 返回 ResultCallback.onSuccess() 回调。 
   - 失败: SDK 返回 ResultCallback.onFailure() 回调及相关错误码 ChannelMessageError 或 PeerMessageError。

3. 将释放 RtmClient 实例资源的方法由 destroy() 更名为 release()。
4. 点对点消息不再通过 rtmMessage 的对象方法创建，改为通过 rtmClient的对象方法创建

5. 从 interface PeerMessageError 中删除枚举 PEER_MESSAGE_RECEIVED_BY_SERVER。

| v0.9                                                         | v0.9.1                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| interface PeerMessageState  <br>int PEER_MESSAGE_INIT = 0; <br>int PEER_MESSAGE_FAILURE = 1; <br>int PEER_MESSAGE_PEER_UNREACHABLE = 2; <br>int PEER_MESSAGE_RECEIVED_BY_PEER = 3; <br>int PEER_MESSAGE_SENT_TIMEOUT = 4; | interface PeerMessageError  <br>int PEER_MESSAGE_ERR_OK = 0; <br>int PEER_MESSAGE_ERR_FAILURE = 1; <br>int PEER_MESSAGE_ERR_TIMEOUT = 2; <br>int PEER_MESSAGE_ERR_PEER_UNREACHABLE = 3; |

6. 从 interface ChannelMessageError 中删除枚举 CHANNEL_MESSAGE_RECEIVED_BY_SERVER。

| v0.9                                                         | v0.9.1                                                       |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| interface ChannelMessageState  <br>int CHANNEL_MESSAGE_INIT = 0; <br>int CHANNEL_MESSAGE_FAILURE = 1; <br>int CHANNEL_MESSAGE_RECEIVED_BY_SERVER = 2; <br>int CHANNEL_MESSAGE_SENT_TIMEOUT = 4; | interface ChannelMessageError <br>int CHANNEL_MESSAGE_ERR_OK = 0; <br>int CHANNEL_MESSAGE_ERR_FAILURE = 1; <br>int CHANNEL_MESSAGE_ERR_TIMEOUT = 2; |

7. 为呼叫邀请功能增加相应方法、回调和枚举，详见 API 参考。 

7.1 LocalInvitation.java
  - public interface LocalInvitation
  - void setContent(String content)
  - String getContent();
  - void setChannelId(String channelId);
  - String getChannelId();
  - String getResponse();
  - int getState();
7.2 RemoteInvitation.java
  - public interface RemoteInvitation 
  - String getCallerId();
  - String getContent();
  - void getChannelId();
  - void setResponse(String response)
  - String getResponse();
  - int getState
7.3 RtmCallEventListener.java
  - public interface RtmCallEventListener 
  - void onLocalInvitationReceivedByPeer;
  - void onLocalInvitationAccepted;
  - void onLocalInvitationRefused;
  - void onLocalInvitationCanceled;
  - void onLocalInvitationFailure;
  - void onRemoteInvitationReceived;
  - void onRemoteInvitationAccepted;
  - void onRemoteInvitationRefused;
  - void onRemoteInvitationCanceled;
  - void onRemoteInvitationFailure;
7.4 RtmCallManager.java
  - public abstract class RtmCallManager 
  - public abstract void setEventListener;
  - public abstract LocalInvitation createLocalInvitation;
  - public abstract void sendLocalInvitation;
  - public abstract void acceptRemoteInvitation;
  - public abstract void refuseRemoteInvitation;
  - public abstract void cancelLocalInvitation;
7.5 RtmStatusCode.java
  - interface LocalInvitationState
  - interface RemoteInvitationState
  - interface LocalInvitationError
  - interface RemoteInvitationError
  - interface InvitationApiCallError


