
---
title: 信令 Server SDK API - Java
description: 
platform: Java
updatedAt: Mon Sep 30 2019 13:22:58 GMT+0800 (CST)
---
# 信令 Server SDK API - Java
> 版本：v1.4.0 BETA

信令 Server SDK 包含以下类:

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>接口类</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><a href="#signal">Signal 类</a></td>
<td>信令的基础类</td>
</tr>
<tr><td><a href="#loginsession">LoginSession 类</a></td>
<td>Signal 类的内部类，在调用 Signal 类的 <code>login</code> 方法时创建 LoginSession 对象。</td>
</tr>
<tr><td><a href="#channel">Channel 类</a></td>
<td>Signal 类的内部类，在调用 LoginSession 对象的 <code>channelJoin</code>方法时创建 Channel 对象。</td>
</tr>
<tr><td><a href="#call">Call 类</a></td>
<td>Signal 类的内部类，在调用 LoginSession 对象的 <code>channelInviteUser2</code> 方法时创建 Call 对象。</td>
</tr>
<tr><td><a href="#messagecallback">MessageCallback 类</a></td>
<td>Signal 类的内部类，用于管理消息相关回调。</td>
</tr>
<tr><td><a href="#logincallback">LoginCallback 类</a></td>
<td>Signal 类的内部类，用于管理登录相关回调。</td>
</tr>
<tr><td><a href="#callcallback">CallCallback 类</a></td>
<td>Signal 类的内部类，用于管理通话相关回调。</td>
</tr>
<tr><td><a href="#channelcallback">ChannelCallback 类</a></td>
<td>Signal 类的内部类，用于管理频道相关回调。</td>
</tr>
</tbody>
</table>



相比其他信令 SDK, Server SDK 允许创建多个 Signal 对象，且每个 Signal 对象包含多个 Session，但目前单个实例只允许同时加入一个频道。

## <a name="signal"></a>Signal 类

Signal 类为信令的基础类。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#signal-java"><span>Signal</span></a></td>
<td>创建 Signal 对象</td>
</tr>
<tr><td><a href="#setdolog-java"><span>setDoLog</span></a></td>
<td>开启打印调试</td>
</tr>
<tr><td><a href="#login-java"><span>public Signal.LoginSession login</span></a></td>
<td><p>登录信令系统</p>
<ul>
<li>成功： 收到 <a href="#onloginsuccess-java"><span>onLoginSuccess</span></a> 回调</li>
<li>失败： 收到 <a href="#onloginfailed-java"><span>onLoginFailed</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#usersetattr-java"><span>userSetAttr</span></a></td>
<td>设置用户属性</td>
</tr>
<tr><td><a href="#usergetattr-java"><span>userGetAttr</span></a></td>
<td>查询用户属性</td>
</tr>
<tr><td><a href="#usergetattrall-java"><span>userGetAttrAll</span></a></td>
<td>查询用户所有属性</td>
</tr>
<tr><td><a href="#channelqueryusernum-java"><span>channelQueryUserNum</span></a></td>
<td>查询指定频道内用户数量</td>
</tr>
<tr><td><a href="#getstatus-java"><span>getStatus</span></a></td>
<td>查询当前登录状态</td>
</tr>
</tbody>
</table>



### <a name="signal-java"></a>创建 Signal 对象 \(Signal\)

```
public Signal(String appid)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>appid</code></td>
<td>声网提供的 App 账号。详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>App ID</span></a>。</td>
</tr>
</tbody>
</table>



### <a name="setdolog-java"></a>开启打印调试 \(setDoLog\)

```
public void setDoLog(boolean doLog)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>doLog</code></td>
<td><ul>
<li>true: 开启打印调试</li>
<li>false: 不开启打印调试</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="login-java"></a>登录 \(login\)

```
public Signal.LoginSession login(String account, String token, Signal.LoginCallback cb)
```

该方法允许用户登录 Agora 信令系统并返回一个 LoginSession 实例对象。用户在进行任何操作以前，必须先登录。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>用户登录厂商 app 的账号，最大 128 字节可见字符（不能使用空格）。可以是用户的 uid、昵称、guid 等任何内容，但必须保证唯一。本文提到的所有 account 参数都是如此。</td>
</tr>
<tr><td><code>token</code></td>
<td>由 App ID 和 App Certificate 生成的 SignalingToken，详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>密钥说明</span></a>。</td>
</tr>
<tr><td><code>cb</code></td>
<td>LoginCallback 实例对象。</td>
</tr>
</tbody>
</table>



> 在测试环境下您可以将参数 token 设为 `_no_need_token` 表示不使用秘钥，但是声网不建议在生产环境下不使用动态秘钥。 默认情况下如果当前已经处于登录状态，调用 <code>login</code> 方法会被忽略。如果希望踢掉老的登录，可以在 <code>login</code> 之前调用 <code>logout</code> 且不用等退出成功就可登录。

### <a name="usersetattr-java"></a>设置用户属性 \(userSetAttr\)

```
public void userSetAttr(String name, String value, final Signal.UserAttrCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>属性名</td>
</tr>
<tr><td><code>value</code></td>
<td>属性值</td>
</tr>
<tr><td><code>cb</code></td>
<td>Signal.UserAttrCallback 实例对象</td>
</tr>
</tbody>
</table>



### <a name="usergetattr-java"></a>查询用户属性 \(userGetAttr\)

```
public void userGetAttr(String account, String name, final Signal.UserAttrCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>用户账号</td>
</tr>
<tr><td><code>name</code></td>
<td>属性名</td>
</tr>
<tr><td><code>cb</code></td>
<td>Signal.UserAttrCallback 实例对象</td>
</tr>
</tbody>
</table>



### <a name="usergetattrall-java"></a>查询用户所有属性 \(userGetAttrAll\)

```
public void userGetAttrAll(String account, final Signal.UserAttrCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>用户账号</td>
</tr>
<tr><td><code>cb</code></td>
<td>Signal.UserGetAttrCallback 实例对象</td>
</tr>
</tbody>
</table>



### <a name="channelqueryusernum-java"></a>查询指定频道内用户数量 \(channelQueryUserNum\)

```
public void channelQueryUserNum(String name, final Signal.UserAttrCallback cb)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>频道名</td>
</tr>
<tr><td><code>cb</code></td>
<td>Signal.UserAttrCallback 实例对象</td>
</tr>
</tbody>
</table>



### <a name="getstatus-java"></a>查询当前登录状态 \(getStatus\)

```
public int getStatus()
```

返回值表示了当前登录状态，对应关系如下：

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>返回值</strong></td>
<td><strong>当前登录状态</strong></td>
</tr>
<tr><td>0</td>
<td>未登录 / 已登出</td>
</tr>
<tr><td>1</td>
<td>连接中 / 登录中</td>
</tr>
<tr><td>2</td>
<td>已登录</td>
</tr>
<tr><td>3</td>
<td>登录失败</td>
</tr>
<tr><td>4</td>
<td>初始状态</td>
</tr>
</tbody>
</table>



## <a name="loginsession"></a>LoginSession 类

Signal 类的内部类。在调用 Signal 类的 <code>login</code> 方法时创建 LoginSession 对象。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#logout-java"><span>logout</span></a></td>
<td>登出信令系统。登出成功将收到 <a href="#onlogout-java"><span>onLogout</span></a> 回调。</td>
</tr>
<tr><td><a href="#messageinstantsend-java"><span>messageInstantSend</span></a></td>
<td><p>向名为 account 的用户发送点对点消息</p>
<ul>
<li>成功： 自己收到 <a href="#onmessagesendsuccess-java"><span>onMessageSendSuccess</span></a> 回调， 消息接收方收到 <a href="#onmessageinstantreceive-java"><span>onMessageInstantReceive</span></a> 回调；</li>
<li>失败： 自己收到 <a href="#onmessagesenderror-java"><span>onMessageSendError</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#messageinstantsend2-java"><span>messageInstantSend</span></a></td>
<td><p>向名为 account 的用户发送点对点消息</p>
<ul>
<li>成功： 自己收到 <a href="#onmessagesendsuccess-java"><span>onMessageSendSuccess</span></a> 回调， 消息接收方收到 <a href="#onmessageinstantreceive-java"><span>onMessageInstantReceive</span></a> 回调；</li>
<li>失败： 自己收到 <a href="#onmessagesenderror-java"><span>onMessageSendError</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#channelinviteuser2-java"><span>channelInviteUser2</span></a></td>
<td><p>邀请名为 account 的用户加入指定频道，呼叫方可以附带一段额外信息。</p>
<ul>
<li>成功： 自己收到 <a href="#oninviteacceptedbypeer-java"><span>onInviteAcceptedByPeer</span></a> 回调， 受邀用户收到 <a href="#oninvitereceived-java"><span>onInviteReceived</span></a> 回调；</li>
<li>失败： 自己收到 <a href="#oninvitefailed-java"><span>onInviteFailed</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#channeljoin-java"><span>channelJoin</span></a></td>
<td><p>加入指定频道。</p>
<ul>
<li>成功： 自己收到 <a href="#onchanneljoined-java"><span>onChannelJoined</span></a> 回调，同频道其他用户收到 <a href="#onchanneluserjoined-java"><span>onChannelUserJoined</span></a> 回调；</li>
<li>失败： 自己收到 <a href="#onchanneljoinfailed-java"><span>onChannelJoinFailed</span></a> 回调。</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="logout-java"></a>退出登录 \(logout\)

```
public void logout();
```

成功退出 Agora 信令系统时会触发 <code>onLogout</code> 回调。

### <a name="messageinstantsend-java"></a>发送点对点消息 \(messageInstantSend\)

```
public void messageInstantSend(String account, String msg, Signal.MessageCallback cb)
```

该方法发送点对点消息到某个指定账号。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>用户登录厂商 app 的账号</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文。每条消息最大为 8 K 字节可见字符</td>
</tr>
<tr><td><code>cb</code></td>
<td>MessageCallback 实例对象</td>
</tr>
</tbody>
</table>



### <a name="messageinstantsend2-java"></a>发送点对点消息 \(messageInstantSend\)

```
public void messageInstantSend(String account, String msg, int ttl, Signal.MessageCallback cb)
```

该方法发送点对点消息到某个指定账号。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>account</code></td>
<td>用户登录厂商 app 的账号</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文。每条消息最大为 8 K 字节可见字符</td>
</tr>
<tr><td><code>ttl</code></td>
<td>生存时间</td>
</tr>
<tr><td><code>cb</code></td>
<td>MessageCallback 实例对象</td>
</tr>
</tbody>
</table>



### <a name="channelinviteuser2-java"></a>发起呼叫 \(channelInviteUser2\)

```
public Signal.LoginSession.Call channelInviteUser2(String channel, String peer, String extra, final Signal.CallCallback cb)
```

该方法用于发起呼叫，即邀请某用户加入某个频道。呼叫和加入频道，是两个独立的过程。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>peer</code></td>
<td>对方用户的账号</td>
</tr>
<tr><td><code>extra</code></td>
<td><p>主叫方想传递给呼叫方的其他信息，最大为 8K 字节可见字符。必须为 JSON 格式。如：</p>
<div><ul>
<li>{“_require_peer_online”:1}  如果对方不在线，则立即触发 <code>onInviteFailed</code> 回调</li>
<li>{“_require_peer_online”:0}  如果对方不在线超过 20 秒，则触发 <code>onInviteFailed</code> 回调（默认）</li>
<li>{“_timeout”:30} 呼叫超时。超时后收到 <code>inviteFailed</code> 的同时消息被销毁，被叫不再收到。范围：0～30s。</li>
<li>{“destMediaUid” : 123}  指定的加入相应媒体频道的 uid。</li>
<li>{“srcNum” : “+123456789”} 指定的在远端手机屏幕上显示的手机号码。</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>cb</code></td>
<td>CallCallback 实例对象</td>
</tr>
</tbody>
</table>



> SIP 呼叫时你不需要设置 `_require_peer_online` 字段。

### <a name="channeljoin-java"></a>加入频道 \(channelJoin\)

```
public Signal.LoginSession.Channel channelJoin(String name, Signal.ChannelCallback cb)
```

该方法让用户加入指定频道。用户一次只能加入一个频道。如加入指定频道时已在其他频道中，将自动从其他频道退出。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channelID</td>
<td><p>频道名。最大为 128 字节可见字符。包含以下特殊频道名或者特殊频道属性（已废弃, 不建议使用）：</p>
<div><ul>
<li><code>__agora_user_online</code> ：当前 <code>appId</code> 中所有用户登录或离线事件将发送至该频道。</li>
<li><code>__agora_channel_event</code> : 当前 <code>appId</code> 中用户加入或离开频道的所有事件将发送至该频道。</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>cb</code></td>
<td>ChannelCallback 实例对象</td>
</tr>
</tbody>
</table>



> 上表 channelID 中列出为特殊频道名，仅供服务端使用。加入相关频道即可获取相关事件，如登录事件、频道事件。

## <a name="channel"></a>Channel 类

Signal 类的内部类。在调用 LoginSession 对象的 <code>channelJoin</code> 方法时创建 Channel 对象。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#channelleave-java"><span>channelLeave</span></a></td>
<td><p>退出指定频道。若退出成功：</p>
<ul>
<li>频道内所有用户都将收到 <a href="#onchanneluserleaved-java"><span>onChannelUserLeaved</span></a> 回调</li>
<li>自己收到 <a href="#onchannelleaved-java"><span>onChannelLeaved</span></a> 回调</li>
</ul>
</td>
</tr>
<tr><td><a href="#channelsetattr-java"><span>channelSetAttr(String name, String value)</span></a></td>
<td>设置频道属性。设置成功将收到 <a href="#onchannelattrupdated-java"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
<tr><td><a href="#channeldelattr-java"><span>channelDelAttr(String name)</span></a></td>
<td>删除频道属性。删除成功将收到 <a href="#onchannelattrupdated-java"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelclearattr-java"><span>channelClearAttr</span></a></td>
<td>删除所有频道属性。删除成功将收到 <a href="#onchannelattrupdated-java"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
<tr><td><a href="#messagechannelsend-java"><span>messageChannelSend(String msg)</span></a></td>
<td><p>发送频道消息（消息发送者必须在频道内）</p>
<ul>
<li>成功： 自己收到 <a href="#onmessagesendsuccess-java"><span>onMessageSendSuccess</span></a> 回调，频道内所有用户收到 <a href="#onmessagechannelreceive-java"><span>onMessageChannelReceive</span></a> 回调；</li>
<li>失败： 自己收到 <a href="#onmessagesenderror-java"><span>onMessageSendError</span></a> 回调。</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="channelleave-java"></a>离开频道 \(channelLeave\)

```
public void channelLeave()
```

该方法用于退出当前的频道。退出成功后，所有频道用户将收到回调 <code>onChannelUserLeaved</code> 。

### <a name="channelsetattr-java"></a>设置频道属性 \(channelSetAttr\)

```
public void channelSetAttr(String name, String value)
```

该方法用于设置频道属性。当操作成功，所有频道用户都将收到 <code>onChannelAttrUpdated</code> 回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>属性名，最大可为 128 字节可见字符</td>
</tr>
<tr><td><code>value</code></td>
<td>属性值，最大可为 8096 字节可见字符</td>
</tr>
</tbody>
</table>

> 以下是 参数 name 的内置属性： 
>
> - `_userNotification` <b>已废弃</b> 1: 默认值，频道发送用户加入或离开频道的事件；0: 频道不发送用户加入或离开频道的事件。 
> - `_channel_ttl` ：频道最后一个用户离开后，多久销毁，单位为秒，默认为7200 。特殊频道永不销毁。 
> - `_member_num` ： 表示当前频道人数，由系统自动更新；更新频次由 `_auto_update_num` 确定。如果  `_auto_update_num` 时间内没有用户进出频道，不会收到人数更新回调  <code>onChannelAttrUpdated</code>；如果 `_auto_update_num` 内有用户频繁进出频道，只会按固定时间收到一次回调。
> - `_auto_update_num` ：表示是否由系统自动更新频道人数。0：关闭（默认）；1-n：（每多少秒更新一次）
> - `_total_member_num` ：<b>已废弃</b> 表示当前频道累计登录人次，由系统自动更新。`_auto_update_num` 与 `_auto_update_total_num` 需要同时设置为非0。更新原则类似 `_member_num`
> - `_auto_update_total_num` : <b>已废弃</b> 表示是否由系统自动更新频道人数，0：关闭（默认）；非0：每多少秒后台更新一次（与 `_auto_update_num` 相同）。
> - `_sendmsg_limit` : 整个频道每秒能发送的消息数
> - `_setattr_limit` : 频道每个属性每秒能修改的次数

### <a name="channeldelattr-java"></a>删除频道属性 \(channelDelAttr\)

```
public void channelDelAttr(String name)
```

该方法删除当前频道的指定属性。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>name</code></td>
<td>属性名，最大可为 128 字节可见字符</td>
</tr>
</tbody>
</table>



### <a name="channelclearattr-java"></a>删除所有频道属性 \(channelClearAttr\)

```
public void channelClearAttr()
```

该方法删除当前频道的所有属性。

### <a name="messagechannelsend-java"></a>发送频道消息 \(messageChannelSend\)

```
public void messageChannelSend(String msg)
```

该方法向同一频道的所有用户发送频道消息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文。每条频道消息最大为 8K 字节可见字符。每个用户每秒不能发超过 60 条消息，整个频道每秒不能发超过 200 条消息。</td>
</tr>
</tbody>
</table>



## <a name="call"></a>Call 类

Signal 类的内部类。在调用 LoginSession 对象的 <code>channelInviteUser2</code> 方法时创建 Call 对象。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#channelinviteaccept-java"><span>channelInviteAccept</span></a></td>
<td>接受来自 account 用户的加入指定频道的呼叫邀请。接收后主叫方将收到 <a href="#oninviteacceptedbypeer-java"><span>onInviteAcceptedByPeer</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinviteaccept2-java"><span>channelInviteAccept</span></a></td>
<td>接受来自 account 用户的加入指定频道的呼叫邀请。接收后主叫方将收到 <a href="#oninviteacceptedbypeer-java"><span>onInviteAcceptedByPeer</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinviterefuse-java"><span>channelInviteRefuse</span></a></td>
<td>拒绝来自 account 用户的加入指定频道的呼叫邀请。拒绝后主叫方将收到 <a href="#oninviterefusedbypeer-java"><span>onInviteRefusedByPeer</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinviterefuse2-java"><span>channelInviteRefuse</span></a></td>
<td>拒绝来自 account 用户的加入指定频道的呼叫邀请。拒绝后主叫方将收到 <a href="#oninviterefusedbypeer-java"><span>onInviteRefusedByPeer</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinviteend-java"><span>channelInviteEnd</span></a></td>
<td>终止向 account 用户发送加入指定频道的邀请。终止成功后主叫方将收到 <a href="#oninviteendbymyself-java"><span>onInviteEndByMyself</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinviteend2-java"><span>channelInviteEnd</span></a></td>
<td>终止向 account 用户发送加入指定频道的邀请。终止成功后主叫方将收到 <a href="#oninviteendbymyself-java"><span>onInviteEndByMyself</span></a> 回调。</td>
</tr>
</tbody>
</table>



### <a name="channelinviteaccept-java"></a>接受呼叫邀请 \(channelInviteAccept\)

```
public void channelInviteAccept()
```

该方法接受呼叫邀请。

### <a name="channelinviteaccept2-java"></a>接受呼叫邀请 \(channelInviteAccept\)

```
public void channelInviteAccept(String extra)
```

该方法接受呼叫邀请。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>extra</code></td>
<td>主叫方想传递给呼叫方的更多信息，可以是任何信息。例如:该呼叫为语音通话或视频通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



### <a name="channelinviterefuse-java"></a>拒绝呼叫请求 \(channelInviteRefuse\)

```
public void channelInviteRefuse()
```

该方法拒绝呼叫请求。调用该 API 后，对方对收到 <code>onInviteRefusedByPeer</code> 事件。

### <a name="channelinviterefuse2-java"></a>拒绝呼叫请求 \(channelInviteRefuse\)

```
public void channelInviteRefuse(String extra)
```

该方法拒绝呼叫请求。调用该 API 后，对方对收到 <code>onInviteRefusedByPeer</code> 事件。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>extra</code></td>
<td>主叫方想传递给呼叫方的更多信息，可以是任何信息。例如:该呼叫为语音通话或视频通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



### <a name="channelinviteend-java"></a>结束呼叫 \(channelInviteEnd\)

```
public void channelInviteEnd()
```

该方法用于当呼叫接通后，调用本接口关闭呼叫。本端会回调 <code>onInviteEndByMyself</code> ，对端会回调 <code>onInviteEndByPeer</code> 。

### <a name="channelinviteend-java"></a>结束呼叫 \(channelInviteEnd\)

```
public void channelInviteEnd(String extra)
```

该方法用于当呼叫接通后，调用本接口关闭呼叫。本端会回调 <code>onInviteEndByMyself</code> ，对端会回调 <code>onInviteEndByPeer</code> 。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>extra</code></td>
<td>主叫方想传递给呼叫方的更多信息，可以是任何信息。例如:该呼叫为语音通话或视频通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



## <a name="userattrcallback"></a>UserAttrCallback 类

UserAttrCallback 的内部类。用于管理消息相关回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#onusergetattr-java"><span>onUserGetAttr</span></a></td>
<td>查询用户属性回调</td>
</tr>
<tr><td><a href="#onusergetattrall-java"><span>onUserGetAttrAll</span></a></td>
<td>查询用户所有属性回调</td>
</tr>
<tr><td><a href="#onusersetattr-java"><span>onUserSetAttr</span></a></td>
<td>设置用户属性回调</td>
</tr>
</tbody>
</table>



### <a name="onusergetattr-java"></a>查询用户属性回调 \(onUserGetAttr\)

```
public void onUserGetAttr(String err, JSONObject ret)
```

当查询用户属性时触发。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>err</code></td>
<td>错误信息。空字符串表示没有发生错误。</td>
</tr>
<tr><td><code>ret</code></td>
<td><ul>
<li>成功： 收到 ret 为: {“result”: “ok”, “account”: “user account”, “name”: “attr name”, “value”: “attr value”}</li>
<li>失败： 收到 ret 为: {“result”: “failed”, “reason”: “failed reason”}</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="onusergetattrall-java"></a>查询用户所有属性值回调 \(onUserGetAttrAll\)

```
public void onUserGetAttrAll(String err, JSONObject ret)
```

当查询用户所有属性值时触发。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>err</code></td>
<td>错误信息。空字符串表示没有发生错误。</td>
</tr>
<tr><td><code>ret</code></td>
<td><ul>
<li>成功： 收到 ret 为: {“result”: “ok”, “account”: “user account”, “json”: {“attr1”:”value1”,”attr2”:”value2”}}</li>
<li>失败： 收到 ret 为: {“result”: “failed”, “reason”: “failed reason”}</li>
</ul>
</td>
</tr>
</tbody>
</table>



### <a name="onusersetattr-java"></a>设置用户属性回调 \(onUserSetAttr\)

```
public void onUserSetAttr(String err, JSONObject ret)
```

当设置用户属性时触发。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>err<code></td>
<td>错误信息。空字符串表示没有发生错误。</td>
</tr>
<tr><td><code>ret</code></td>
<td><ul>
<li>成功： 收到 ret 为: {“result”: “ok”, “reason”:”“}</li>
<li>失败： 收到 ret 为: {“result”: “failed”, “reason”: “failed reason”}</li>
</ul>
</td>
</tr>
</tbody>
</table>



## <a name="logincallback"></a>LoginCallback 类

Signal 类的内部类。用于管理登录相关回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#onloginsuccess-java"><span>onLoginSuccess</span></a></td>
<td>登录成功回调</td>
</tr>
<tr><td><a href="#onlogout-java"><span>onLogout</span></a></td>
<td>退出登录回调</td>
</tr>
<tr><td><a href="#onloginfailed-java"><span>onLoginFailed</span></a></td>
<td>登录失败回调</td>
</tr>
<tr><td><a href="#onmessageinstantreceive-java"><span>onMessageInstantReceive</span></a></td>
<td>接收方收到消息时接收方收到的回调</td>
</tr>
<tr><td><a href="#oninvitereceived-java"><span>onInviteReceived</span></a></td>
<td>收到呼叫邀请回调</td>
</tr>
<tr><td><a href="#onerror-java"><span>onError</span></a></td>
<td>出错回调</td>
</tr>
<tr><td><a href="#onchanneljoined-java"><span>onChannelJoined</span></a></td>
<td>加入频道回调</td>
</tr>
<tr><td><a href="#onchanneljoinfailed-java"><span>onChannelJoinFailed</span></a></td>
<td>加入频道失败回调</td>
</tr>
<tr><td><a href="#onchannelleaved-java"><span>onChannelLeaved</span></a></td>
<td>离开频道回调</td>
</tr>
</tbody>
</table>



### <a name="onloginsuccess-java"></a>登录成功回调 \(onLoginSuccess\)

```
public void onLoginSuccess(Signal.LoginSession session, int uid)
```

当登录成功后触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>uid</code></td>
<td>系统自动生成的内部用户 ID</td>
</tr>
</tbody>
</table>



### <a name="onlogout-java"></a>退出登录回调 \(onLogout\)

```
public void onLogout(Signal.LoginSession session, int ecode)
```

当退出登录时触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



### <a name="onloginfailed-java"></a>登录失败回调 \(onLoginFailed\)

```
public void onLoginFailed(Signal.LoginSession session, int ecode)
```

当登录失败时触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



### <a name="onmessageinstantreceive-java"></a>接收方收到消息时接收方收到的回调 \(onMessageInstantReceive\)

```
public void onMessageInstantReceive(Signal.LoginSession session, String account, int uid, String msg)


```

接收方收到消息时接收方收到的回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>account</code></td>
<td>用户登录厂商 app 的账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>系统自动生成的内部用户 ID</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文</td>
</tr>
</tbody>
</table>



### <a name="oninvitereceived-java"></a>收到呼叫邀请回调 \(onInviteReceived\)

```
public void onInviteReceived(Signal.LoginSession session, Signal.LoginSession.Call call)


```

当对方收到呼叫邀请触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>call</code></td>
<td>Call 的实例对象</td>
</tr>
</tbody>
</table>



### <a name="onerror-java"></a>错误报告回调 \(onError\)

```
public void onError(Signal.LoginSession session, int ecode, String reason)


```

当出错时会触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误码</td>
</tr>
<tr><td><code>reason</code></td>
<td>错误原因</td>
</tr>
</tbody>
</table>



## <a name="channelcallback"></a>ChannelCallback 类

ChannelCallback 类是 Signal 类的内部类，用于管理频道相关回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#onchanneljoinfailed-java"><span>onChannelJoinFailed</span></a></td>
<td>加入频道失败回调</td>
</tr>
<tr><td><a href="#onchannelleaved-java"><span>onChannelLeaved</span></a></td>
<td>离开频道回调</td>
</tr>
<tr><td><a href="#onchanneluserjoined-java"><span>onChannelUserJoined</span></a></td>
<td>其他用户加入频道回调</td>
</tr>
<tr><td><a href="#onchanneluserleaved-java"><span>onChannelUserLeaved</span></a></td>
<td>其他用户离开频道回调</td>
</tr>
<tr><td><a href="#onchanneluserlist-java"><span>onChannelUserList</span></a></td>
<td>获取频道内用户列表回调</td>
</tr>
<tr><td><a href="#onchannelattrupdated-java"><span>onChannelAttrUpdated</span></a></td>
<td>频道属性发生变化回调</td>
</tr>
<tr><td><a href="#onchannelqueryusernum-java"><span>onChannelQueryUserNum</span></a></td>
<td>查询频道内用户数回调</td>
</tr>
<tr><td><a href="#onmessagechannelreceive-java"><span>onMessageChannelReceive</span></a></td>
<td>收到频道消息回调</td>
</tr>
</tbody>
</table>



### <a name="onchanneljoined-java"></a>加入频道回调 \(onChannelJoined\)

```
public void onChannelJoined(Signal.LoginSession session, Signal.LoginSession.Channel channelName)


```

当加入频道成功时触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名称</td>
</tr>
</tbody>
</table>



### <a name="onchanneljoinfailed-java"></a>加入频道失败回调 \(onChannelJoinFailed\)

```
public void onChannelJoinFailed(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode)


```

当加入频道失败触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名称</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



### <a name="onchannelleaved-java"></a>离开频道回调 \(onChannelLeaved\)

```
public void onChannelLeaved(Signal.LoginSession session, Signal.LoginSession.Channel channel, int ecode)


```

当离开频道成功触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话实例</td>
</tr>
<tr><td><code>channel</code></td>
<td>频道实例</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



### <a name="onchanneluserjoined-java"></a>其他用户加入频道回调 \(onChannelUserJoined\)

```
public void onChannelUserJoined(Signal.LoginSession session, Signal.LoginSession.Channel channel, String account, int uid)


```

当有用户加入频道触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名称</td>
</tr>
<tr><td><code>account</code></td>
<td>用户登录厂商 app 的账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>系统生成的内部 uid</td>
</tr>
</tbody>
</table>



### <a name="onchanneluserleaved-java"></a>其他用户离开频道回调 \(onChannelUserLeaved\)

```
public void onChannelUserLeaved(Signal.LoginSession session, Signal.LoginSession.Channel channel, String account, int uid)


```

当有用户调用 <code>channelLeave</code> 成功时触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名称</td>
</tr>
<tr><td><code>account</code></td>
<td>用户登录厂商 app 的账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>系统生成的内部 uid</td>
</tr>
</tbody>
</table>



### <a name="onchanneluserlist-java"></a>获取频道内用户列表回调 \(onChannelUserList\)

```
public void onChannelUserList(Signal.LoginSession session, Signal.LoginSession.Channel channel, List<String> users, List<Integer> uids)


```

当加入频道成功后，本人会收到此回调。

> 返回的频道内用户列表为最近的 200 个用户。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名称</td>
</tr>
<tr><td><code>users</code></td>
<td>用户登录厂商 app 的账号列表</td>
</tr>
<tr><td><code>uids</code></td>
<td>频道内部 uid 列表</td>
</tr>
</tbody>
</table>



### <a name="onchannelattrupdated-java"></a>频道属性发生变化回调 \(onChannelAttrUpdated\)

```
public void onChannelAttrUpdated(Signal.LoginSession session, Signal.LoginSession.Channel channel, String name, String value, String type)


```

当频道属性变化时触发。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名称</td>
</tr>
<tr><td><code>name</code></td>
<td>属性名</td>
</tr>
<tr><td><code>value</code></td>
<td>属性值</td>
</tr>
<tr><td><code>type</code></td>
<td>属性类型</td>
</tr>
</tbody>
</table>



### <a name="onchannelqueryusernum-java"></a>查询频道内用户数回调 \(onChannelQueryUserNum\)

```
public void onChannelQueryUserNum(Signal.LoginSession session, String err, int num)


```

查询频道用户数量时触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>err</code></td>
<td>错误信息。空字符串表示没有发生错误。</td>
</tr>
<tr><td><code>num</code></td>
<td>频道内用户数量。可以为 0。</td>
</tr>
</tbody>
</table>



### <a name="onmessagechannelreceive-java"></a>收到频道消息回调 \(onMessageChannelReceive\)

```
public void onMessageChannelReceive(Signal.LoginSession session, Signal.LoginSession.Channel channel, String account, int uid, String msg)


```

当收到频道消息时触发。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>channel</code></td>
<td>频道名称</td>
</tr>
<tr><td><code>account</code></td>
<td>用户登录厂商 app 的账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>系统生成的内部 uid</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文</td>
</tr>
</tbody>
</table>



## <a name="messagecallback"></a>MessageCallback 类

MessageCallback 的内部类。用于管理消息相关回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#onmessagesendsuccess-java"><span>onMessageSendSuccess</span></a></td>
<td>消息已发送成功回调</td>
</tr>
<tr><td><a href="#onmessagesenderror-java"><span>onMessageSendError</span></a></td>
<td>消息发送失败回调</td>
</tr>
</tbody>
</table>



### <a name="onmessagesendsuccess-java"></a>消息已发送回调 \(onMessageSendSuccess\)

```
public void onMessageSendSuccess(Signal.LoginSession session)


```

当发送消息成功时触发。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
</tbody>
</table>



### <a name="onmessagesenderror-java"></a>消息发送失败回调 \(onMessageSendError\)

```
public void onMessageSendError(Signal.LoginSession session, int ecode)


```

当消息发送失败时触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



## <a name="callcallback"></a>CallCallback 类

CallCallback 类是 Signal 类的内部类，用于管理呼叫相关回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#oninvitereceivedbypeer-java"><span>onInviteReceivedByPeer</span></a></td>
<td>远端已收到呼叫回调</td>
</tr>
<tr><td><a href="#oninviteacceptedbypeer-java"><span>onInviteAcceptedByPeer</span></a></td>
<td>远端已接受呼叫回调</td>
</tr>
<tr><td><a href="#oninviterefusedbypeer-java"><span>onInviteRefusedByPeer</span></a></td>
<td>对方已拒绝呼叫回调</td>
</tr>
<tr><td><a href="#oninvitefailed-java"><span>onInviteFailed</span></a></td>
<td>呼叫失败回调</td>
</tr>
<tr><td><a href="#oninviteendbypeer-java"><span>onInviteEndByPeer</span></a></td>
<td>对方已结束呼叫回调</td>
</tr>
<tr><td><a href="#oninviteendbymyself-java"><span>onInviteEndByMyself</span></a></td>
<td>本地已结束呼叫回调</td>
</tr>
<tr><td><a href="#oninvitemsg-java"><span>onInviteMsg</span></a></td>
<td>本地已收到消息回调</td>
</tr>
</tbody>
</table>



### <a name="oninvitereceivedbypeer-java"></a>远端已收到呼叫回调 \(onInviteReceivedByPeer\)

```
public void onInviteReceivedByPeer(Signal.LoginSession session, Signal.LoginSession.Call call)


```

当呼叫被对方收到时触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>call</code></td>
<td>呼叫对象本身</td>
</tr>
</tbody>
</table>



### <a name="oninviteacceptedbypeer-java"></a>远端已接受呼叫回调 \(onInviteAcceptedByPeer\)

```
public void onInviteAcceptedByPeer(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

当呼叫被对方接受时触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>call</code></td>
<td>呼叫对象本身</td>
</tr>
<tr><td><code>extra</code></td>
<td>主叫方想传递给呼叫方的更多信息，可以是任何信息。例如:该呼叫为语音通话或视频通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



### <a name="oninviterefusedbypeer-java"></a>对方已拒绝呼叫回调 \(onInviteRefusedByPeer\)

```
public void onInviteRefusedByPeer(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

当呼叫被对方拒绝时触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>call</code></td>
<td>呼叫对象本身</td>
</tr>
<tr><td><code>extra</code></td>
<td>主叫方想传递给呼叫方的更多信息，可以是任何信息。例如:该呼叫为语音通话或视频通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



### <a name="oninvitefailed-java"></a>呼叫失败回调 \(onInviteFailed\)

```
public void onInviteFailed(Signal.LoginSession session, Signal.LoginSession.Call call, int ecode)


```

当呼叫失败时触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>call</code></td>
<td>呼叫对象本身</td>
</tr>
<tr><td><code>extra</code></td>
<td>主叫方想传递给呼叫方的更多信息，可以是任何信息。例如:该呼叫为语音通话或视频通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



### <a name="oninviteendbypeer-java"></a>对方已结束呼叫回调 \(onInviteEndByPeer\)

```
public void onInviteEndByPeer(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

当呼叫被对方结束时触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>call</code></td>
<td>呼叫对象本身</td>
</tr>
<tr><td><code>extra</code></td>
<td>主叫方想传递给呼叫方的更多信息，可以是任何信息。例如:该呼叫为语音通话或视频通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



### <a name="oninviteendbymyself-java"></a>本地已结束呼叫回调 \(onInviteEndByMyself\)

```
public void onInviteEndByMyself(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

当呼叫被自己结束时触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>call</code></td>
<td>呼叫对象本身</td>
</tr>
<tr><td><code>extra</code></td>
<td>主叫方想传递给呼叫方的更多信息，可以是任何信息。例如:该呼叫为语音通话或视频通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



### <a name="oninvitemsg-java"></a>本地已收到对方消息回调 \(onInviteMsg）

```
public void onInviteMsg(Signal.LoginSession session, Signal.LoginSession.Call call, String extra)


```

当本地收到对方发的 DTMF 消息时会触发该回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>session</code></td>
<td>会话名称</td>
</tr>
<tr><td><code>call</code></td>
<td>呼叫对象本身</td>
</tr>
<tr><td><code>extra</code></td>
<td>发送方想给本地发送的更多消息，必须为 JSON 格式</td>
</tr>
</tbody>
</table>



## 错误代码和警告代码

详见 [错误代码和警告代码](../../cn/API%20Reference/the_error_signaling.md)。
