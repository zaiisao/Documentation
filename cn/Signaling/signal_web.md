
---
title: 信令 API
description: 
platform: Web
updatedAt: Mon Nov 18 2019 06:17:00 GMT+0800 (CST)
---
# 信令 API
> 版本：v1.4.0 BETA

信令 Web API 包含以下类：

- [Signal 类](#signal)
- [Session 类](#session)
- [Call 类](#call)
- [Channel 类](#channel)

## <a name="signal"></a>Signal 类

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#login-web"><span>login</span></a></td>
<td><p>登录信令系统</p>
<ul>
<li>成功： 收到 Session 类的 <a href="#onloginsuccess-web"><span>onLoginSuccess</span></a> 回调</li>
<li>失败： 收到 Session 类的 <a href="#onloginfailed-web"><span>onLoginFailed</span></a> 回调</li>
<li>登录之后失去与服务器的连接： 收到 Session 类的 <a href="#onlogout-web"><span>onLogout</span></a> 回调</li>
</ul>
</td>
</tr>
<tr><td><a href="#setdolog-web"><span>setDoLog</span></a></td>
<td>SDK 打开日志</td>
</tr>
<tr><td><a href="#getsdkversion-web"><span>getSDKVersion</span></a></td>
<td>查询 SDK 版本</td>
</tr>
</tbody>
</table>



根据以下获取 Signal 对象:

```
signal = Signal(appid)
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><code>appid</code></td>
<td>appid, 该项目的 App ID。关于如何获取 App ID, 详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>App ID</span></a>。</td>
</tr>
</tbody>
</table>



### 方法

#### <a name="login-web"></a>登录 \(login\)

```
login(account, token, reconnect_count, reconnect_time) : Session
```

使用该方法登录 Agora 信令系统，它返回一个 session 。您可以在 Session 中设置回调，完成消息、频道、呼叫等操作。

- 登录成功：回调 <code>onLoginSuccess</code> ，
- 登录失败：回调 <code>onLoginFailed</code> ，
- 登录之后失去与服务器的连接：回调 <code>onLogout</code> 。

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
<td>用户登录厂商 app 的账号，最大 128 字节可见字符（不能使用空格）。可以是用户的 uid、昵称、guid 等任何内容，但必须保证唯一。</td>
</tr>
<tr><td><code>token</code></td>
<td>由 App ID 和 App 证书生成的 SignalingToken，详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>密钥说明</span></a>。</td>
</tr>
<tr><td><code>reconnect_count</code></td>
<td>最高重连次数（默认 10 次）</td>
</tr>
<tr><td><code>reconnect_time</code></td>
<td>最高重连时间 (默认 30 秒)</td>
</tr>
</tbody>
</table>



> 在测试环境下您可以将参数 token 设为 `_no_need_token` 表示不使用秘钥，但是声网不建议在生产环境下不使用动态秘钥。 默认情况下如果当前已经处于登录状态，调用 <code>login</code> 方法会被忽略。如果希望踢掉老的登录，可以在 <code>login</code> 之前调用 <code>logout</code> 且不用等退出成功就可登录。 与系统失去链接后，当 `reconnect_count` 或 `reconnect_time` 任一项达到设置值时，系统退出重连。

#### <a name="setdolog-web"></a>SDK 打开日志（setDoLog）

```
setDoLog(isEnabled, level)
```

该方法用于设置日志打印。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>isEnabled</code></td>
<td><p>是否打印日志</p>
<div><ul>
<li>true: 打印日志</li>
<li>false: 不打印日志（默认）</li>
</ul>
</div>
</td>
</tr>
<tr><td><code>level</code></td>
<td><p>日志分级打印</p>
<div><ul>
<li>DEBUG（默认）</li>
<li>WARNING</li>
<li>INFO</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>


#### <a name="getsdkversion-web"></a>查询 SDK 版本 \(getSDKVersion\)

```
getSDKVersion() : String
```

使用该方法查询 SDK 版本。

该请求是同步请求。返回值是版本号的字符串，如 “1.3.0” 。

## <a name="session"></a>Session 类

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#invoke-web"><span>invoke(func, args, cb)</span></a></td>
<td>RPC 远程过程调用方法。可用于用户或频道相关操作。</td>
</tr>
<tr><td><a href="#messageinstantsend-web"><span>messageInstantSend</span></a></td>
<td><p>向名为 account 的用户发送点对点消息</p>
<div><ul>
<li>成功：消息接收方收到 Session 类的 <a href="#onmessageinstantreceive-web"><span>onMessageInstantReceive</span></a> 回调；</li>
</ul>
</div>
</td>
</tr>
<tr><td><a href="#logout-web"><span>logout</span></a></td>
<td>登出信令系统。登出成功将收到 Session 类的 <a href="#onlogout-web"><span>onLogout</span></a> 回调。</td>
</tr>
<tr><td><a href="#channeljoin-web"><span>channelJoin</span></a></td>
<td><p>加入指定频道。</p>
<div><ul>
<li>成功：自己收到 Channel 类的 <a href="#onchanneljoined-web"><span>onChannelJoined</span></a> 回调，同频道其他用户收到 Channel 类的 <a href="#onchanneluserjoined-web"><span>onChannelUserJoined</span></a> 回调；</li>
<li>失败：自己收到 Channel 类的 <a href="#onchanneljoinfailed-web"><span>onChannelJoinFailed</span></a> 回调。</li>
</ul>
</div>
</td>
</tr>
<tr><td><a href="#channelinviteuser2-web"><span>channelInviteUser2</span></a></td>
<td><p>邀请名为 peer 的用户加入指定频道，呼叫方可以附带一段额外信息。</p>
<div><ul>
<li>成功： 自己收到 Call 类的 <a href="#oninviteacceptedbypeer-web"><span>onInviteAcceptedByPeer</span></a> 回调， 受邀用户收到 Call 类的 <a href="#oninvitereceived-web"><span>onInviteReceived</span></a> 回调；</li>
<li>失败： 自己收到 Call 类的 <a href="#oninvitefailed-web"><span>onInviteFailed</span></a> 回调。</li>
</ul>
</div>
</td>
</tr>
<tr><td><a href="#getstatus-web"><span>getStatus</span></a></td>
<td>查询当前登录状态</td>
</tr>
</tbody>
</table>



<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#onloginsuccess-web"><span>onLoginSuccess</span></a></td>
<td>登录成功回调</td>
</tr>
<tr><td><a href="#onloginfailed-web"><span>onLoginFailed</span></a></td>
<td>登录失败回调</td>
</tr>
<tr><td><a href="#onlogout-web"><span>onLogout</span></a></td>
<td>退出登录回调</td>
</tr>
<tr><td><a href="#onmessageinstantreceive-web"><span>onMessageInstantReceive</span></a></td>
<td>接收方收到消息时接收方收到的回调</td>
</tr>
<tr><td><a href="#oninvitereceived-web"><span>onInviteReceived</span></a></td>
<td>收到呼叫邀请回调</td>
</tr>
</tbody>
</table>



### 方法

#### <a name="invoke-web"></a>RPC 远程过程调用 \(invoke\)

```
invoke(func, args, cb)
```

该方法用于远程过程调用。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>实现功能</strong></td>
<td><strong>参数设置及返回值</strong></td>
</tr>
<tr><td>查询用户状态</td>
<td><ul>
<li>func: io.agora.signal.user_query_user_status</li>
<li>args: {“account”:用户名, JSON 字符串}</li>
<li>返回值：status(1:在线；0:不在线)</li>
</ul>
</td>
</tr>
<tr><td>设置用户属性</td>
<td><ul>
<li>func: io.agora.signal.user_set_attr</li>
<li>args: {“name”:属性名,”value”:属性值}</li>
</ul>
</td>
</tr>
<tr><td>获取用户属性</td>
<td><ul>
<li>func: io.agora.signal.user_get_attr</li>
<li>args: {“account”:用户名, JSON 字符串,”name”:属性名}</li>
<li>返回值: {“value”:属性值}</li>
</ul>
</td>
</tr>
<tr><td>获取用户所有属性</td>
<td><ul>
<li>func: io.agora.signal.user_get_attr_all</li>
<li>args: {“account”:用户名, JSON 字符串}</li>
<li>返回值：所有属性的 JSON 值</li>
</ul>
</td>
</tr>
<tr><td>查询频道人数</td>
<td><ul>
<li>func: io.agora.signal.channel_query_num</li>
<li>args: {“name”:频道名}</li>
<li>返回值：总人数</li>
</ul>
</td>
</tr>
<tr><td>查询是否在频道内</td>
<td><ul>
<li>func: io.agora.signal.channel_query_user_isin</li>
<li>args: {“name”:频道名,”account”:用户名, JSON 字符串}</li>
<li>返回值：isin（1: 在频道内；0:不在频道内）</li>
</ul>
</td>
</tr>
<tr><td>查询频道用户列表</td>
<td><ul>
<li>func: io.agora.signal.channel_query_userlist</li>
<li>args: {“name”:频道名}</li>
<li>返回值：{“num”:总人数,”list”:最近成员}</li>
</ul>
</td>
</tr>
<tr><td>查询频道最近用户列表</td>
<td><b>已废弃</b><ul>
<li>func: io.agora.signal.channel_query_userlist_all</li>
<li>args: {“name”:频道名,”num”:数量(默认 1000*1000)}</li>
<li>返回值：{“num”:总人数,”list”:最近成员}</li>
<li>建议服务端使用，客户端使用注意频率限制</li>
</ul>
</td>
</tr>
<tr><td>清空频道属性</td>
<td><ul>
<li>func: io.agora.signal.channel_clear_attr</li>
<li>args: {“channel”:频道名}</li>
</ul>
</td>
</tr>
<tr><td>删除频道属性</td>
<td><ul>
<li>func: io.agora.signal.channel_del_attr</li>
<li>args: {“channel”:频道名,”name”:属性名}</li>
</ul>
</td>
</tr>
<tr><td>设置频道属性</td>
<td><ul>
<li>func: io.agora.signal.channel_set_attr</li>
<li>args: {“channel”:频道名,”name”:属性名,”value”:属性值}</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### <a name="messageinstantsend-web"></a>发送点对点消息 \(messageInstantSend\)

```
messageInstantSend(peer, msg, cb)
```

该方法发送点对点消息到某个指定账号。当对方账号收到消息时将收到回调 <code>onMessageInstantReceive</code> 。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>peer</code></td>
<td>对方的账号</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文。每条消息最大为 8196 字节可见字符</td>
</tr>
<tr><td><code>cb</code></td>
<td>当该方法执行成功时的回调</td>
</tr>
</tbody>
</table>



#### <a name="logout-web"></a>退出 \(logout\)

```
logout()
```

该方法用于退出 Agora 信令系统。成功退出 Agora 信令系统时会触发 <code>onLogout</code> 回调。

#### <a name="channeljoin-web"></a>加入频道 \(channelJoin\)

```
channelJoin(name)
```

使用该方法加入频道，它返回一个频道对象。

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
<td><p>频道名。最大为 128 字节可见字符。包含以下特殊频道名或者特殊频道属性（已废弃, 不建议使用）：</p>
<div><ul>
<li><code>__agora_user_online</code> ：当前 appId 中所有用户登录或离线事件将发送至该频道。</li>
<li><code>__agora_channel_event</code> : 当前 appId 中用户加入或离开频道的所有事件将发送至该频道。</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### <a name="channelinviteuser2-web"></a>发起呼叫 \(channelInviteUser2\)

```
channelInviteUser2(channelID, peer, extra) : Call
```

该方法用于发起呼叫，即邀请某用户加入某个频道。

如果呼叫失败，会回调 <code>onInviteFailed</code>。可能的原因有：

- 对方不在线
- 本端网络不通
- 服务器异常

如果收到对方的确认信息，本地将收到回调 <code>onInviteReceivedByPeer</code>, 对方会收到回调 <code>onInviteReceived</code>。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>channelID</code></td>
<td>频道名</td>
</tr>
<tr><td><code>peer</code></td>
<td>对方的账号</td>
</tr>
<tr><td><code>extra</code></td>
<td><p>本次呼叫的其他信息，最大为 8K 字节可见字符。必须为 JSON 格式。如：</p>
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
</tbody>
</table>



> SIP 呼叫时你不需要设置 `_require_peer_online` 字段。


#### <a name="getstatus-web"></a>查询当前登录状态 \(getStatus\)

```
getStatus() : Integer
```

使用该方法查询当前登录状态。

该请求是同步请求。返回值表示了当前登录状态，对应关系如下：

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
<td>未登录</td>
</tr>
<tr><td>1</td>
<td>连接中</td>
</tr>
<tr><td>2</td>
<td>已登录</td>
</tr>
</tbody>
</table>


### 回调

#### <a name="onloginsuccess-web"></a>登录成功回调 \(onLoginSuccess\)

```
onLoginSuccess(uid)
```

登录成功时触发本回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
</tbody>
</table>




#### <a name="onloginfailed-web"></a>登录失败回调 \(onLoginFailed\)

```
onLoginFailed(ecode)
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
<tr><td><code>ecode</code></td>
<td><p>错误代码</p>
<ul>
<li>SUCCESS = 0,</li>
<li>LOGOUT_E_OTHER = 100</li>
<li>LOGOUT_E_USER = 101,  // logout by user</li>
<li>LOGOUT_E_NET = 102,  // network failure</li>
<li>LOGOUT_E_KICKED = 103,  // login in other device</li>
<li>LOGOUT_E_PACKET = 104,</li>
<li>LOGOUT_E_TOKENEXPIRED = 105, // token expired</li>
<li>LOGOUT_E_OLDVERSION = 106,</li>
<li>LOGOUT_E_TOKENWRONG = 107,</li>
<li>LOGOUT_E_ALREADY_LOGOUT = 108,</li>
<li>LOGIN_E_OTHER = 200,</li>
<li>LOGIN_E_NET = 201,</li>
<li>LOGIN_E_FAILED = 202,</li>
<li>LOGIN_E_CANCEL = 203,</li>
<li>LOGIN_E_TOKENEXPIRED = 204,</li>
<li>LOGIN_E_OLDVERSION = 205,</li>
<li>LOGIN_E_TOKENWRONG = 206,</li>
<li>LOGIN_E_TOKEN_KICKED = 207,</li>
<li>LOGIN_E_ALREADY_LOGIN = 208,</li>
<li>JOINCHANNEL_E_OTHER = 300,</li>
<li>SENDMESSAGE_E_OTHER = 400,</li>
<li>SENDMESSAGE_E_TIMEOUT = 401,</li>
<li>QUERYUSERNUM_E_OTHER = 500,</li>
<li>QUERYUSERNUM_E_TIMEOUT = 501,</li>
<li>QUERYUSERNUM_E_BYUSER = 502,</li>
<li>LEAVECHANNEL_E_OTHER = 600,</li>
<li>LEAVECHANNEL_E_KICKED = 601,</li>
<li>LEAVECHANNEL_E_BYUSER = 602,</li>
<li>LEAVECHANNEL_E_LOGOUT = 603,</li>
<li>LEAVECHANNEL_E_DISCONN  = 604,</li>
<li>INVITE_E_OTHER = 700,</li>
<li>INVITE_E_REINVITE = 701,</li>
<li>INVITE_E_NET = 702,</li>
<li>INVITE_E_PEEROFFLINE = 703,</li>
<li>INVITE_E_TIMEOUT = 704,</li>
<li>INVITE_E_CANTRECV = 705,</li>
<li>GENERAL_E = 1000,</li>
<li>GENERAL_E_FAILED = 1001,</li>
<li>GENERAL_E_UNKNOWN = 1002,</li>
<li>GENERAL_E_NOT_LOGIN = 1003,</li>
<li>GENERAL_E_WRONG_PARAM = 1004,</li>
<li>GENERAL_E_LARGE_PARAM   = 1005</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### <a name="onlogout-web"></a>用户登出回调 \(onLogout\)

```
onLogout(reason)
```

当用户离线 \(登出 Agora 信令系统\) 时触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



#### <a name="onmessageinstantreceive-web"></a>接收方收到消息时接收方收到的回调 \(onMessageInstantReceive\)

```
onMessageInstantReceive(account, uid, msg)
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
<tr><td><code>account</code></td>
<td>消息发送方的账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>msg</code></td>
<td>收到的即时消息正文</td>
</tr>
</tbody>
</table>



#### <a name="oninvitereceived-web"></a>收到呼叫邀请回调 \(onInviteReceived\)

```
onInviteReceived(call)
```

当收到 呼叫<call> 邀请触发该回调。

## <a name="call"></a>Call 类

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#channelinviteaccept-web"><span>channelInviteAccept</span></a></td>
<td>接受来自 account 用户的加入指定频道的呼叫邀请。接收后主叫方将收到 <a href="#oninviteacceptedbypeer-web"><span>onInviteAcceptedByPeer</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinviterefuse-web"><span>channelInviteRefuse</span></a></td>
<td>拒绝来自 account 用户的加入指定频道的呼叫邀请。拒绝后主叫方将收到 <a href="#oninviterefusedbypeer-web"><span>onInviteRefusedByPeer</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinvitedtmf-web"><span>channelInviteDTMF(dtmf)</span></a></td>
<td>发送 DTMF 消息到对端 phoneNum 用户，一般用于 SIP 网关的呼叫。</td>
</tr>
<tr><td><a href="#channelinviteend-web"><span>channelInviteEnd</span></a></td>
<td>终止向 account 用户发送加入指定频道的邀请。终止成功后主叫方将收到 <a href="#oninviteendbymyself-web"><span>onInviteEndByMyself</span></a> 回调。</td>
</tr>
</tbody>
</table>



<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#oninvitereceivedbypeer-web"><span>onInviteReceivedByPeer</span></a></td>
<td>远端已收到呼叫回调</td>
</tr>
<tr><td><a href="#oninviteacceptedbypeer-web"><span>onInviteAcceptedByPeer</span></a></td>
<td>远端已接受呼叫回调</td>
</tr>
<tr><td><a href="#oninviterefusedbypeer-web"><span>onInviteRefusedByPeer</span></a></td>
<td>对方已拒绝呼叫回调</td>
</tr>
<tr><td><a href="#oninvitefailed-web"><span>onInviteFailed</span></a></td>
<td>呼叫失败回调</td>
</tr>
<tr><td><a href="#oninviteendbypeer-web"><span>onInviteEndByPeer</span></a></td>
<td>对方已结束呼叫回调</td>
</tr>
<tr><td><a href="#oninviteendbymyself-web"><span>onInviteEndByMyself</span></a></td>
<td>本地已结束呼叫回调</td>
</tr>
<tr><td><a href="#oninvitemsg-web"><span>onInviteMsg</span></a></td>
<td>本地已收到消息回调</td>
</tr>
</tbody>
</table>



### 方法

#### <a name="channelinviteaccept-web"></a>接受呼叫邀请 \(channelInviteAccept\)

```
channelInviteAccept(extra)
```

该方法接受呼叫邀请。主叫方会回调 <code>onInviteAcceptedByPeer</code> 。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空</td>
</tr>
</tbody>
</table>



#### <a name="channelinviterefuse-web"></a>拒绝呼叫邀请 \(channelInviteRefuse\)

```
channelInviteRefuse(extra)
```

该方法用于当收到呼叫邀请时，调用本接口拒绝收到的呼叫。对方会收到 <code>onInviteRefusedByPeer</code> 的回调。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空。</td>
</tr>
</tbody>
</table>



#### <a name="channelinvitedtmf-web"></a>向远端发送 DTMF 消息 \(channelInviteDTMF\)

```
channelInviteDTMF(dtmf)
```

该方法发送 DTMF 消息到对端，用于 SIP 网关的呼叫。对端会回调 <code>onInviteMsg</code> 。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>dtmf</code></td>
<td>拨通后，后续需要输入的模拟电话按键 (DTMF)。</td>
</tr>
</tbody>
</table>



#### <a name="channelinviteend-web"></a>结束呼叫 \(channelInviteEnd\)

```
channelInviteEnd()
```

该方法用于当呼叫接通后，调用本接口结束呼叫。本端会回调 <code>onInviteEndByMyself</code>，对端会回调 <code>onInviteEndByPeer</code>。

### 回调

#### <a name="oninvitereceivedbypeer-web"></a>远端已收到呼叫回调 \(onInviteReceivedByPeer\)

```
onInviteReceivedByPeer(extra)
```

当对方收到呼叫时触发该回调。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空。</td>
</tr>
</tbody>
</table>





#### <a name="oninviteacceptedbypeer-web"></a>远端已接受呼叫回调 \(onInviteAcceptedByPeer\)

```
onInviteAcceptedByPeer(extra)
```

当呼叫被对方接受时触发。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空。</td>
</tr>
</tbody>
</table>



#### <a name="oninviterefusedbypeer-web"></a>对方已拒绝呼叫回调 \(onInviteRefusedByPeer\)

```
onInviteRefusedByPeer(extra)
```

当呼叫被对方拒绝时触发。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空。</td>
</tr>
</tbody>
</table>



#### <a name="oninvitefailed-web"></a>呼叫失败回调 \(onInviteFailed\)

```
onInviteFailed(extra)
```

当呼叫失败时触发。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空。</td>
</tr>
</tbody>
</table>



#### <a name="oninviteendbypeer-web"></a>对方已结束呼叫回调 \(onInviteEndByPeer\)

```
onInviteEndByPeer(extra)
```

当呼叫被对方结束时触发。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空。</td>
</tr>
</tbody>
</table>



#### <a name="oninviteendbymyself-web"></a>本地已结束呼叫回调 \(onInviteEndByMyself\)

```
onInviteEndByMyself(extra)
```

当本地结束呼叫时触发该回调。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空。</td>
</tr>
</tbody>
</table>



#### <a name="oninvitemsg-web"></a>本地已收到消息回调 \(onInviteMsg\)

```
onInviteMsg(extra)
```

当本地接收到远端用户发送的消息时触发该回调。

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
<td>关于该呼叫的其他信息, 可以是一个字符串，一个 JSON 字符串或将其设置为空。</td>
</tr>
</tbody>
</table>



## <a name="channel"></a>Channel 类

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#messagechannelsend-web"><span>messageChannelSend</span></a></td>
<td><p>发送频道消息（消息发送者必须在频道内）</p>
<div><ul>
<li>成功： 频道内所有用户收到 Channel 类的 <a href="#onmessagechannelreceive-web"><span>onMessageChannelReceive</span></a> 回调；</li>
</ul>
</div>
</td>
</tr>
<tr><td><a href="#channelleave-web"><span>channelLeave</span></a></td>
<td><p>退出指定频道。退出成功时：</p>
<div><ul>
<li>频道内所有用户都将收到 <a href="#onchanneluserleaved-web"><span>onChannelUserLeaved</span></a> 回调</li>
<li>自己收到 <a href="#onchannelleaved-web"><span>onChannelLeaved</span></a> 回调</li>
</ul>
</div>
</td>
</tr>
<tr><td><a href="#channelsetattr-web"><span>channelSetAttr</span></a></td>
<td>设置频道属性。设置成功将收到 Channel 类的 <a href="#onchannelattrupdated-web"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
<tr><td><a href="#channeldelattr-web"><span>channelDelAttr</span></a></td>
<td>删除频道属性。删除成功将收到 Channel 类的 <a href="#onchannelattrupdated-web"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelclearattr-web"><span>channelClearAttr</span></a></td>
<td>删除所有频道属性。删除成功将收到 Channel 类的 <a href="#onchannelattrupdated-web"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
</tbody>
</table>



<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#onchanneljoined-web"><span>onChannelJoined</span></a></td>
<td>加入频道回调</td>
</tr>
<tr><td><a href="#onchanneljoinfailed-web"><span>onChannelJoinFailed</span></a></td>
<td>加入频道失败回调</td>
</tr>
<tr><td><a href="#onchannelleaved-web"><span>onChannelLeaved</span></a></td>
<td>离开频道回调</td>
</tr>
<tr><td><a href="#onchanneluserjoined-web"><span>onChannelUserJoined</span></a></td>
<td>其他用户加入频道回调</td>
</tr>
<tr><td><a href="#onchanneluserleaved-web"><span>onChannelUserLeaved</span></a></td>
<td>其他用户离开频道回调</td>
</tr>
<tr><td><a href="#onchanneluserlist-web"><span>onChannelUserList</span></a></td>
<td>获取频道内用户列表回调</td>
</tr>
<tr><td><a href="#onchannelattrupdated-web"><span>onChannelAttrUpdated</span></a></td>
<td>频道属性发生变化回调</td>
</tr>
<tr><td><a href="#onmessagechannelreceive-web"><span>onMessageChannelReceive</span></a></td>
<td>收到频道消息回调</td>
</tr>
</tbody>
</table>



### 方法

#### <a name="messagechannelsend-web"></a>发送频道消息 \(messageChannelSend\)

```
messageChannelSend(msg, cb)
```

该方法发送频道消息，且同一频道内的所有用户均会收到 <code>onMessageChannelReceive</code> 回调。

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
<td>频道消息正文。如果是大规模群组通话，每条消息最大为 8K 字节可见字符。每个用户每秒不能发超过 60 条消息，整个频道每秒不能发超过 200 条消息。</td>
</tr>
<tr><td><code>cb</code></td>
<td>当该方法执行成功时的回调。</td>
</tr>
</tbody>
</table>



代码示例:

```
channel.messageChannelSend( "message", function(){
  // channel message was sent
});
```

#### <a name="channelleave-web"></a>离开频道 \(channelLeave\)

```
channelLeave(cb)
```

该方法用于退出当前的频道。退出成功后，频道内的所有其他用户将收到 <code>onChannelUserLeaved</code> 回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>cb</code></td>
<td>当该方法执行成功时的回调</td>
</tr>
</tbody>
</table>



#### <a name="channelsetattr-web"></a>设置频道属性 \(channelSetAttr\)

```
channelSetAttr(name, value, cb)


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
<td>属性名称。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>value</code></td>
<td>属性值。最大为 8096 字节可见字符</td>
</tr>
<tr><td><code>cb</code></td>
<td>当该方法执行成功时的回调</td>
</tr>
</tbody>
</table>



> 以下是 参数 name 的内置属性： 
>
> - `_userNotification` <b>该属性已废弃</b> 1: 默认值，频道发送用户加入或离开频道的事件；0: 频道不发送用户加入或离开频道的事件。 
> - `_channel_ttl` ：频道最后一个用户离开后，多久销毁，单位为秒，默认为7200 。特殊频道永不销毁。 
> - `_member_num` ： 表示当前频道人数，由系统自动更新；更新频次由 `_auto_update_num` 确定。如果  `_auto_update_num` 时间内没有用户进出频道，不会收到人数更新回调  <code>onChannelAttrUpdated</code>；如果 `_auto_update_num` 内有用户频繁进出频道，只会按固定时间收到一次回调。
> - `_auto_update_num` ：表示是否由系统自动更新频道人数。0：关闭（默认）；1-n：（每多少秒更新一次）
> - `_total_member_num` ：<b>该属性已废弃</b> 表示当前频道累计登录人次，由系统自动更新。`_auto_update_num` 与 `_auto_update_total_num` 需要同时设置为非0。更新原则类似 `_member_num`
> - `_auto_update_total_num` : <b>该属性已废弃</b> 表示是否由系统自动更新频道人数，0：关闭（默认）；非0：每多少秒后台更新一次（与 `_auto_update_num` 相同）。
> - `_sendmsg_limit` : 整个频道每秒能发送的消息数
> - `_setattr_limit` : 频道每个属性每秒能修改的次数

代码示例:

```
channel.channelSetAttr("name", "john", function(){
  // attr name was set


```

\}\);

#### <a name="channeldelattr-web"></a>删除频道属性 \(channelDelAttr\)

```
channelDelAttr(name, cb)


```

该方法用于删除频道的某一属性。

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
<td>属性名称。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>cb</code></td>
<td>当该方法执行成功时的回调</td>
</tr>
</tbody>
</table>



代码示例:

```
channel.channelDelAttr("name", function(){
  // attr name was deleted
});


```

#### <a name="channelclearattr-web"></a>删除所有频道属性 \(channelClearAttr\)

```
channelClearAttr(cb)


```

该方法用于删除指定频道的所有属性。当操作成功，所有频道用户都将收到 <code>onChannelAttrUpdated</code> 回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>cb</code></td>
<td>当该方法执行成功时的回调</td>
</tr>
</tbody>
</table>



代码示例:

```
channel.channelClearAttr(function(){
  // attr was cleared
});


```

### 回调

#### <a name="onchanneljoined-web"></a>加入频道回调 \(onChannelJoined\)

```
onChannelJoined()


```

当加入频道成功时触发此回调。

#### <a name="onchanneljoinfailed-web"></a>加入频道失败回调 \(onChannelJoinFailed\)

```
onChannelJoinFailed(ecode)


```

当加入频道失败时触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



#### <a name="onchannelleaved-web"></a>离开频道回调 \(onChannelLeaved\)

```
onChannelLeaved(ecode)


```

当离开频道时触发此回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误代码</td>
</tr>
</tbody>
</table>



#### <a name="onchanneluserjoined-web"></a>其他用户加入频道回调 \(onChannelUserJoined\)

```
onChannelUserJoined(account, uid)


```

当有其他用户加入频道触发该回调。

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
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
</tbody>
</table>



#### <a name="onchanneluserleaved-web"></a>其他用户离开频道回调 \(onChannelUserLeaved\)

```
onChannelUserLeaved(account, uid)


```

当有用户离开频道触发此回调。

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
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
</tbody>
</table>



#### <a name="onchanneluserlist-web"></a>获取频道内用户列表回调 \(onChannelUserList\)

```
onChannelUserList(users)


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
<tr><td><code>users</code></td>
<td>成功加入频道后，获取的频道内用户列表。格式为: <code>[[account1, uid1], [account2, uid2], …, [accountn, uidn]]</code></td>
</tr>
</tbody>
</table>



#### <a name="onchannelattrupdated-web"></a>频道属性发生变化回调 \(onChannelAttrUpdated\)

```
onChannelAttrUpdated(name, value, type)


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
<td>属性名称。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>value</code></td>
<td>属性值。最大为 8096 字节可见字符</td>
</tr>
<tr><td><code>type</code></td>
<td><p>变化类型:</p>
<div><ul>
<li>“update” : 更新</li>
<li>“del”: 删除</li>
<li>“clear”: 全部删除</li>
<li>“set”: 废弃字段</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### <a name="onmessagechannelreceive-web"></a>收到频道消息回调 \(onMessageChannelReceive\)

```
onMessageChannelReceive(account, uid, msg)


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
<tr><td><code>account</code></td>
<td>用户登录厂商 app 的账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文</td>
</tr>
</tbody>
</table>



## 错误代码和警告代码

详见 [错误代码和警告代码](../../cn/API%20Reference/the_error_signaling.md)。
