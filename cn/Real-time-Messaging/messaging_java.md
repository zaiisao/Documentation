
---
title: 收发点对点消息和频道消息
description: 
platform: Linux Java
updatedAt: Fri Jul 10 2020 06:40:29 GMT+0800 (CST)
---
# 收发点对点消息和频道消息

本页介绍在正式使用 [Agora RTM Linux Java SDK](https://docs.agora.io/cn/Real-time-Messaging/downloads) 进行实时消息通讯前，需要准备的开发环境要求及 SDK 集成方法等内容。

## Demo 体验


你可以到 GitHub 下载最新版的 [Agora-RTM-Tutorial-Java](https://github.com/AgoraIO/RTM/tree/master/Agora-RTM-Tutorial-Java) 查看完整的源码和代码逻辑：

- [RtmJavaDemo.java](https://github.com/AgoraIO/RTM/blob/master/Agora-RTM-Tutorial-Java/src/main/java/io/agora/RtmJavaDemo.java)


## 开发环境要求



- Ubuntu 14.04 LTS 64 位或 CentOS 7.0+ 64 位。
- glibc 2.14 及以上。
- IDE 工具 Eclipse。

- 一个有效的 [Agora 开发者账号](https://sso.agora.io/en/signup)。

<div class="alert note">如果你的网络环境部署了防火墙，请根据<a href="https://docs.agora.io/cn/Agora%20Platform/firewall?platform=All%20Platforms">应用企业防火墙限制</a >打开相关端口并设置域名白名单。</div> 



## 准备开发环境

开发环境的准备包含以下内容：

- [获取 App ID](#appid)
- [快速集成 SDK](#sdk)

### <a name="appid"></a> 获取 App ID


参考以下步骤获取一个 App ID。若已有App ID，可以直接查看[快速集成 SDK](#sdk)。
<details>
	<summary><font color="#3ab7f8">获取 App ID</font></summary>

1. 进入[控制台](https://console.agora.io/)，并按照屏幕提示注册账号并登录控制台。详见[创建新账号](../../cn/Real-time-Messaging/sign_in_and_sign_up.md)。

2. 点击左侧导航栏的 ![](https://web-cdn.agora.io/docs-files/1551254998344) 图标进入[项目管理](https://console.agora.io/projects)页面，点击**创建**按钮。

![](https://web-cdn.agora.io/docs-files/1574156100068)

3. 在弹出的对话框内输入**项目名称**，选择 App ID 作为**鉴权机制**，再点击“提交”。

![](https://web-cdn.agora.io/docs-files/1574921599254)

4. 项目创建成功后，你会在**项目列表**下看到刚刚创建的项目，并找到对应的 App ID。

![](https://web-cdn.agora.io/docs-files/1574921811175)



</details>

### <a name="sdk"></a> 快速集成 SDK



- 下载最新版的 <a href="https://docs.agora.io/cn/Real-time-Messaging/downloads">Agora RTM Linux Java SDK</a> 并解压。
- 将解压后的 SDK 包的 **libs** 文件夹下的 **.so** 库文件复制到系统的默认库搜索路径，通常是 **/usr/lib**。
- 将解压后的 SDK 包的 **libs** 文件夹下的 **.jar** 复制到你自己项目的 *lib* 文件夹。
- 选中 **.jar** 包点击鼠标右键： **Build Path > Add to Build Path** 把 **.jar** 添加到项目路径。
	<i> 成功添加后，你可以在 <b>Referenced Libraries</b> 文件夹下看到对该 **.jar** 包的引用。</i>



## 实时消息和基本频道操作


本节主要提供实现实时消息和基本频道操作的 API 调用时序图及相关示例代码。

### API 调用时序图

#### 登录登出 Agora RTM 系统

![](https://web-cdn.agora.io/docs-files/1583998128489)

#### 收发点对点消息


![](https://web-cdn.agora.io/docs-files/1583942421049)

#### 加入离开频道

![](https://web-cdn.agora.io/docs-files/1583942482690)

#### 收发频道消息

![](https://web-cdn.agora.io/docs-files/1583942536983)



### 初始化

在创建实例前，请确保你已完成环境准备，安装包获取等步骤。

创建实例需要填入准备好的 `App ID`, 只有 `App ID` 相同的应用才能互通。

指定事件回调 `RtmClientListener`，SDK 通过该回调通知应用程序：

- SDK 与 RTM 系统的连接状态变化；
- 接收点对点消息；
- 其他相关事件。

```java
import io.agora.rtm.ErrorInfo;
import io.agora.rtm.ResultCallback;
import io.agora.rtm.RtmChannel;
import io.agora.rtm.RtmChannelListener;
import io.agora.rtm.RtmChannelMember;
import io.agora.rtm.RtmClient;
import io.agora.rtm.RtmClientListener;
import io.agora.rtm.RtmMessage;
import io.agora.rtm.SendMessageOptions;

public void init() {
		try {
				mRtmClient = RtmClient.createInstance(APPID,
												new RtmClientListener() {
						@Override
						public void onConnectionStateChanged(int state, int reason) {
								Log.d(TAG, "Connection state changes to "
										+ state + " reason: " + reason);
						}

						@Override
						public void onMessageReceived(RtmMessage rtmMessage, String peerId) {
								String msg = rtmMessage.getText();
								Log.d(TAG, "Message received " + " from " + peerId + msg 
														);
						}
				});
		} catch (Exception e) {
				Log.d(TAG, "RTM SDK init fatal error!");
				throw new RuntimeException("You need to check the RTM init process.");
		}
}
```


### 登录

APP 必须在登录 RTM 服务器之后，才可以使用 RTM 的点对点消息和群聊功能。在此之前，请确保 `RtmClient` 初始化完成。

- 传入能标识用户角色和权限的 Token。如果安全要求不高，也可以将值设为 `null`。Token 需要在应用程序的服务器端生成，具体生成办法，详见 [校验用户权限](../../cn/Real-time-Messaging/rtm_token.md)。
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

### 点对点消息

在成功登录 RTM 服务器之后，可以开始使用 RTM 的点对点消息功能。

#### 发送点对点消息

调用 `sendMessageToPeer` 方法发送点对点消息。在该方法中：

- 传入目标消息接收方的用户 ID。
- 传入 `RtmMessage` 对象实例。该消息对象由 `RtmClient` 类的的 `createMessage()` 实例方法创建，并使用消息实例的 `setText()` 方法设置消息内容。
- 传入消息发送结果监听器，用于接收消息发送结果回调，如：服务器已接收，发送超时，对方不可达等。

```java
public void sendPeerMessage(String dst, String content) {
        
        final RtmMessage message = mRtmClient.createMessage();
        message.setText(content);

        SendMessageOptions option = new SendMessageOptions();
        option.enableOfflineMessaging = true;

        mRtmClient.sendMessageToPeer(dst, message, option, new ResultCallback<Void>() {

            @Override
            public void onSuccess(Void aVoid) {

            }

            @Override
            public void onFailure(ErrorInfo errorInfo) {

            }
        });
    }
```

#### 接收点对点消息

点对点消息的接收通过创建 `RtmClient` 实例的时候传入的 `RtmClientListener` 回调接口进行监听。在该回调接口的 `onMessageReceived(RtmMessage message, String peerId)` 回调方法中：

- 通过 `message.getText()` 方法可以获取到消息文本内容。
- `peerId` 是消息发送方的用户 ID。


### 频道消息

App 在成功登录 RTM 服务器 之后，可以开始使用 RTM 的频道消息功能。

#### 创建 、加入频道实例

- 传入能标识每个频道的 ID。ID 为字符串，不能为空或者多于 64 个字符，也不能是字符串 `"null"`。  
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
						" or already in use. See the API Reference for more information.");
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

#### 收发频道消息

在成功加入频道之后，用户可以开始向该频道发送消息。频道消息的接收通过创建频道消息的时候传入的回调接口进行监听。

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





#### 退出频道

调用实例的 `leave()` 方法可以退出该频道。退出频道之后可以调用 `join()` 方法再重新加入频道。



## 开发注意事项


- RTM 支持多个相互独立的 [RtmClient](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html) 实例。

- 在收发点对点消息或进行其他频道操作前，请确保你已成功登录 Agora RTM 系统（即确保已经收到 [onSuccess](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/interfaceio_1_1agora_1_1rtm_1_1_result_callback.html#a7206b30500655c4a73d146acf50cb6f5)）。

- 使用频道核心功能前必须通过调用 [createChannel](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html#a95ebbd1a1d902572b444fef7853f335a) 方法创建频道实例。
- 你可以创建多个 [RtmClient](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_client.html) 客户端实例，但是每个客户端实例最多只能同时加入 20 个频道。每个频道都应有不同的 `channelId` 参数。

- 当离开了频道且不再加入该频道时，可以调用 [release](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_channel.html#a725d008cb19f44496948ee8f1826deaf) 方法及时释放频道实例所占用的资源。

- 接收到的 [RtmMessage](https://docs.agora.io/cn/Real-time-Messaging/API%20Reference/RTM_java_linux/classio_1_1agora_1_1rtm_1_1_rtm_message.html) 消息对象不能重复利用再用于发送。



