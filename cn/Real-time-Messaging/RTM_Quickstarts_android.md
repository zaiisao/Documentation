
---
title: RTM 快速开始
description: 
platform: Android
updatedAt: Tue Aug 27 2019 03:11:47 GMT+0800 (CST)
---
# RTM 快速开始
## 集成客户端

本章介绍在正式使用 Agora RTM Java SDK for Android 进行实时消息通讯前，需要准备的开发环境，包含前提条件及 SDK 集成方法等内容。

### 前提条件

请确保满足以下开发环境要求：

- Android SDK API Level ≥ 16

- Android Studio 3.0 或以上版本

- App 要求 Android 4.1 或以上设备

- 如果你的 App 以 Android 9 及以上为目标平台，请关注 [Android 隐私权变更](https://developer.android.com/about/versions/pie/android-9.0-changes-28?hl=zh-CN#privacy-changes-p)。



### 创建 Agora 账号并获取 App ID

1. 进入 <https://dashboard.agora.io/> ，按照屏幕提示创建一个开发者账号。
2. 登录 Dashboard 页面，点击添加新项目。
3. 填写项目名， 然后点击提交 。
4. 在你创建的项目下，查看并获取该项目对应的 App ID。

### 安装 SDK

#### 方法 1：通过 JCenter 自动导入

1. 使用 **Android Studio** 在 **app/build.gradle** 文件内添加以下代码（1.0.1 为当前最新版本号）

```java
dependencies {
    ...
    implementation 'io.agora.rtm:rtm-sdk:1.0.1'

}
```

2. 同步项目文件。点击 **Sync Project With Gradle Files** 按钮，直到同步完成。

#### 方法 2：手动导入 SDK 文件

1. 在[SDK 下载](https://docs.agora.io/cn/Real-time-Messaging/downloads)下载最新版的 Agora RTM Java SDK for Android 并解压。
2. 将 SDK 解压文件包 **libs** 文件夹下的以下文件拷贝到你项目的对应文件夹下：

| 文件                                     | 对应项目文件夹                                     |
| ---------------------------------------- | -------------------------------------------------- |
| **agora-rtm_sdk.jar**                    | **~/app/libs/**                                    |
| **/arm64-v8a/libagora-rtm-sdk-jni.so**   | **~/app/libs/arm64-v8a/libagora-rtm-sdk-jni.so**   |
| **/armeabi-v7a/libagora-rtm-sdk-jni.so** | **~/app/libs/armeabi-v7a/libagora-rtm-sdk-jni.so** |
| **/x86/libagora-rtm-jni.so**             | **~/app/libs/x86/libagora-rtm-jni.so**             |
| **/x86_64/libagora-rtm-sdk-jni.so**      | **~/app/x86_64/libagora-rtm-sdk-jni.so**           |




### 防止混淆代码

在 proguard-rules.pro 文件中，为 Agora SDK 添加 -keep 类的配置，这样可以防止混淆 Agora SDK 公共类名称:

`-keep class io.agora.**{*;}`


## 初始化

在创建实例前，请确保你已完成环境准备，安装包获取等步骤。

创建实例需要填入准备好的 `App ID`, 只有 `App ID` 相同的应用才能互通。

指定一个事件回调，SDK 通过回调通知应用程序 SDK 的状态变化和运行事件等，如连接状态变化，消息接收等。

```java
import io.agora.rtm.ErrorInfo;
import io.agora.rtm.ResultCallback;
import io.agora.rtm.RtmChannel;
import io.agora.rtm.RtmChannelListener;
import io.agora.rtm.RtmChannelMember;
import io.agora.rtm.RtmClient;
import io.agora.rtm.RtmClientListener;
import io.agora.rtm.RtmMessage;

public void init() {
		try {
				mRtmClient = RtmClient.createInstance(mContext, APPID,
												new RtmClientListener() {
						@Override
						public void onConnectionStateChanged(int state, int reason) {
								Log.d(TAG, "on connection state changed to "
										+ state + " reason: " + reason);
						}

						@Override
						public void onMessageReceived(RtmMessage rtmMessage, String peerId) {
								String msg = rtmMessage.getText();
								Log.d(TAG, "Receives message: " + msg 
														+ " from " + peerId);
						}
				});
		} catch (Exception e) {
				Log.d(TAG, "RTM SDK init fatal error!");
				throw new RuntimeException("You need to check the RTM init process.");
		}
}
```



### 注意事项

RTM 支持多实例，事件回调须是不同的实例。

当实例不再使用时，可以调用实例的 `release()`方法释放资源。

## 登录

APP 必须在登录 RTM 服务器之后，才可以使用 RTM 的点对点消息和群聊功能。在此之前，请确保 `RtmClient` 初始化完成。

- 传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 `null`。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [校验用户权限](../../cn/Real-time-Messaging/RTM_key.md)。
- 传入能标识每个用户的 ID。用户 ID 为字符串，必须是可见字符（可以带空格），不能为空或者多于 64 个字符，也不能是字符串 `"null"`。
- 传入结果回调，用于接收登录 RTM 服务器成功或者失败的结果回调。

```java
mRtmClient.login(null, userId, new ResultCallback<Void>() {
		@Override
		public void onSuccess(Void responseInfo) {
				loginStatus = true;
				Log.d(TAG, "login success!");
		}
		@Override
		public void onFailure(ErrorInfo errorInfo) {
				loginStatus = false;
				Log.d(TAG, "login failure!");
		}
});

```

如果需要退出登录，可以调用 `logout()` 方法，退出登录之后可以调用 `login()` 重新登录或者切换账号。

```java
mRtmClient.logout(null);
```

## 点对点消息

在成功登录 RTM 服务器之后，可以开始使用 RTM 的点对点消息功能。

### 发送点对点消息

调用 `sendMessageToPeer` 方法发送点对点消息。在该方法中：

- 传入目标消息接收方的用户 ID。
- 传入 `RtmMessage` 对象实例。该消息对象由 `RtmClient` 类的的 `createMessage()` 实例方法创建，并使用消息实例的 `setText()` 方法设置消息内容。
- 传入消息发送结果监听器，用于接收消息发送结果回调，如：服务器已接收，发送超时，对方不可达等。

```java
public void sendPeerMessage(String dst, String message) {
		RtmMessage msg = mRtmClient.createMessage();
		msg.setText(message);

		mRtmClient.sendMessageToPeer(dst, msg, new ResultCallback<Void>() {
				@Override
				public void onSuccess(Void aVoid) {
				}

				@Override
				public void onFailure(ErrorInfo errorInfo) {
						final int errorCode = errorInfo.getErrorCode();
						Log.d(TAG, "Fails to send the message to the peer, errorCode = "
														+ errorCode);
				}
		});
}

```

### 接收点对点消息

点对点消息的接收通过创建 `RtmClient` 实例的时候传入的 `RtmClientListener` 回调接口进行监听。在该回调接口的 `onMessageReceived(RtmMessage message, String peerId)` 回调方法中：

- 通过 `message.getText()` 方法可以获取到消息文本内容。
- `peerId` 是消息发送方的用户 ID。

### 注意事项

- 接收到的 `RtmMessage` 消息对象不能重复利用再用于发送。

## 频道消息

App 在成功登录 RTM 服务器 之后，可以开始使用 RTM 的频道消息功能。

### 创建 、加入频道实例

- 传入能标识每个频道的 ID。ID 为字符串，必须是可见字符（可以带空格），不能为空或者多于 64 个字符，也不能是字符串 `"null"`。  
- 指定一个频道监听器。SDK 通过回调通知应用程序频道的状态变化和运行事件等，如: 接收到频道消息、用户加入和退出频道等。

```java
private RtmChannelListener mRtmChannelListener = new RtmChannelListener() {
    @Override
    public void onMessageReceived(RtmMessage message, RtmChannelMember fromMember) {
        String text = message.getText();
        String fromUser = fromMember.getUserId();
    }
 
    @Override
    public void onMemberJoined(RtmChannelMember member) {
 
    }
 
    @Override
    public void onMemberLeft(RtmChannelMember member) {
 
    }
};
```

```java
try {
		mRtmChannel = mRtmClient.createChannel("demoChannelId", mRtmChannelListener);
} catch (RuntimeException e) {
		Log.e(TAG, "Fails to create channel. Maybe the channel ID is invalid," +
						" or already in use. See the API reference for more information.");
}

		mRtmChannel.join(new ResultCallback<Void>() {
				@Override
				public void onSuccess(Void responseInfo) {
						Log.d(TAG, "Successfully joins the channel!");
				}

				@Override
				public void onFailure(ErrorInfo errorInfo) {
						Log.d(TAG, "join channel failure! errorCode = "
																+ errorInfo.getErrorCode());
				}
		});
```

### 发送频道消息

在成功加入频道之后，用户可以开始向该频道发送消息。

调用频道实例的 `sendChannelMessage()` 方法向该频道发送消息。在该方法中：

- 传入 `RtmMessage` 对象实例。该消息对象由 `RtmClient` 类的 `createMessage()` 实例方法创建，并使用消息实例的 `setText()` 方法设置消息内容。
- 传入消息发送结果监听器，用于接收消息发送结果回调，如：服务器已接收，发送超时等。

```java
public void sendChannelMessage(String msg) {
		RtmMessage message = mRtmClient.createMessage();
		message.setText(msg);

		mRtmChannel.sendMessage(message, new ResultCallback<Void>() {
				@Override
				public void onSuccess(Void aVoid) {
				}

				@Override
				public void onFailure(ErrorInfo errorInfo) {
				}
		});
}
```

### 接收频道消息

频道消息的接收通过创建频道消息的时候传入的回调接口进行监听。

### 获取频道成员列表

调用实例的 `getMembers()` 方法可以获取到当前在该频道内的用户列表。 

```java
public void getChannelMemberList() {
		mRtmChannel.getMembers(new ResultCallback<List<RtmChannelMember>>() {
				@Override
				public void onSuccess(final List<RtmChannelMember> responseInfo) {
				}
				@Override
				public void onFailure(ErrorInfo errorInfo) {
				}
		});
}
```

### 退出频道

调用实例的 `leave()` 方法可以退出该频道。退出频道之后可以调用 `join()` 方法再重新加入频道。

### 注意事项

- 每个客户端都需要首先调用 `createChannel` 方法创建频道实例才能使用频道消息功能，该实例只是本地的一个类对象实例。
- RTM 支持同时创建最多 20 个不同的频道实例并加入到多个频道中，但是每个频道实例必须使用不同的频道 ID 以及不同的回调。
- 如果频道 ID 非法，或者具有相同 ID 的频道实例已经在本地创建，`createChannel` 时将返回 `null`。
- 接收到的 `RtmMessage` 消息对象不能重复利用再用于发送。
- 当离开了频道且不再加入该频道时，可以调用 `RtmChannel` 实例的 `release()` 方法及时释放频道实例所占用的资源。
- 所有回调如无特别说明，除了基本的参数合法性检查失败触发的回调，均为异步调用。
