
---
title: 信令 API
description: 
platform: Android
updatedAt: Mon Nov 25 2019 09:38:56 GMT+0800 (CST)
---
# 信令 API
> 版本：v1.4.4
> 属性名称前如果有下划线表示其为内置属性（而非自定义属性）。

## 全局方法

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#getinstance-android"><span>getInstance()</span></a></td>
<td>获取 AgoraAPIOnlySignal 实例</td>
</tr>
</tr>
<tr><td><a href="#createinstance-android"><span>createAgoraSDKInstance()</span></a></td>
<td>创建多个 AgoraAPIOnlySignal 实例</td>
</tr>
</tbody>
</table>



## IAgoraAPI 接口类

IAgoraAPI 类为信令的接口类。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>方法</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#destroy-android"><span>destroy()</span></a></td>
<td>销毁信令实例</td>
</tr>
<tr><td><a href="#callbackset-android"><span>callbackSet()</span></a></td>
<td>为信令实例设置回调对象</td>
</tr>
<tr><td><a href="#callbackget-android"><span>callbackGet()</span></a></td>
<td>获取信令实例的回调对象</td>
</tr>
<tr><td><a href="#login-android"><span>login()</span></a></td>
<td><p>登录信令系统</p>
<ul>
<li>成功： 收到 <code>ICallBack</code> 类的 <a href="#onloginsuccess-android"><span>onLoginSuccess</span></a> 回调</li>
<li>失败： 收到 <code>ICallBack</code> 类的 <a href="#onloginfailed-android"><span>onLoginFailed</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#login2-android"><span>login2()</span></a></td>
<td><p>登录信令系统</p>
<ul>
<li>成功：收到 <code>ICallBack</code> 类的 <a href="#onloginsuccess-android"><span>onLoginSuccess</span></a> 回调</li>
<li>失败：收到 <code>ICallBack</code> 类的 <a href="#onloginfailed-android"><span>onLoginFailed</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#logout-android"><span>logout()</span></a></td>
<td>登出信令系统。登出成功将收到 ICallBack 类的 <a href="#onlogout-android"><span>onLogout</span></a> 回调。</td>
</tr>
<tr><td><a href="#invoke-android"><span>invoke()</span></a></td>
<td>RPC 远程过程调用方法。可用于用户或频道相关操作。结果通过 <code>ICallBack</code> 类的 <a href="#oninvokeret-android"><span>onInvokeRet</span></a> 回调返回。</td>
</tr>
<tr><td><a href="#queryuserstatus-android"><span>queryUserStatus()</span></a></td>
<td>查询名为 account 的用户是否在线。结果通过 <code>ICallBack</code> 类的 <a href="#onqueryuserstatusresult-android"><span>onQueryUserStatusResult</span></a> 回调返回。</td>
</tr>
<tr><td><a href="#setattr-android"><span>setAttr()</span></a></td>
<td>设置当前登录用户的相关属性值。</td>
</tr>
<tr><td><a href="#getattr-android"><span>getAttr()</span></a></td>
<td>获取当前登录用户的相关属性值。结果通过 ICallBack 类的 <a href="#onuserattrresult-android"><span>onUserAttrResult</span></a> 回调返回。</td>
</tr>
<tr><td><a href="#getattrall-android"><span>getAttrAll()</span></a></td>
<td>获取当前登录用户的全部属性值。结果通过 ICallBack 类的 <a href="#onuserattrallresult-android"><span>onUserAttrAllResult</span></a> 回调返回。</td>
</tr>
<tr><td><a href="#getuserattr-android"><span>getUserAttr()</span></a></td>
<td>获取名为 account 的用户的 name 属性值。结果通过 ICallBack 类的 <a href="#onuserattrresult-android"><span>onUserAttrResult</span></a> 回调返回。</td>
</tr>
<tr><td><a href="#getuserattrall-android"><span>getUserAttrAll()</span></a></td>
<td>获取名为 account 的用户的所有属性值。结果通过 ICallBack 类的 <a href="#onuserattrallresult-android"><span>onUserAttrAllResult</span></a> 回调返回。</td>
</tr>
<tr><td><a href="#messageinstantsend-android"><span>messageInstantSend()</span></a></td>
<td><p>向名为 account 的用户发送点对点消息</p>
<ul>
<li>成功：自己收到 <code>ICallBack</code> 类的 <a href="#onmessagesendsuccess-android"><span>onMessageSendSuccess</span></a> 回调， 消息接收方收到 <code>ICallback</code> 类的 <a href="#onmessageinstantreceive-android"><span>onMessageInstantReceive</span></a> 回调；</li>
<li>失败：自己收到 <code>ICallBack</code> 类的 <a href="#onmessagesenderror-android"><span>onMessageSendError</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#channeljoin-android"><span>channelJoin()</span></a></td>
<td><p>加入指定频道。</p>
<ul>
<li>成功：自己收到 <code>ICallBack</code> 类的 <a href="#onchanneljoined-android"><span>onChannelJoined</span></a> 回调，同频道其他用户收到 <code>ICallback</code> 类的 <a href="#onchanneluserjoined-android"><span>onChannelUserJoined</span></a> 回调；</li>
<li>失败：自己收到 <code>ICallBack</code> 类的 <a href="#onchanneljoinfailed-android"><span>onChannelJoinFailed</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#channelleave-android"><span>channelLeave()</span></a></td>
<td><p>退出指定频道。退出成功时：</p>
<ul>
<li>频道内所有用户都将收到 <a href="#onchanneluserleaved-android"><span>onChannelUserLeaved</span></a> 回调</li>
<li>自己收到 <a href="#onchannelleaved-android"><span>onChannelLeaved</span></a> 回调</li>
</ul>
</td>
</tr>
<tr><td><a href="#channelqueryusernum-android"><span>channelQueryUserNum()</span></a></td>
<td>查询频道用户数。结果通过 <code>ICallBack</code> 类的 <a href="#onchannelqueryusernumresult-android"><span>onChannelQueryUserNumResult</span></a> 回调返回。</td>
</tr>
<tr><td><a href="#channelsetattr-android"><span>channelSetAttr()</span></a></td>
<td>设置频道属性。设置成功将收到 <code>ICallBack</code> 类的 <a href="#onchannelattrupdated-android"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
<tr><td><a href="#channeldelattr-android"><span>channelDelAttr()</span></a></td>
<td>删除频道属性。删除成功将收到 <code>ICallBack</code> 类的 <a href="#onchannelattrupdated-android"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelclearattr-android"><span>channelClearAttr()</span></a></td>
<td>删除所有频道属性。删除成功将收到 <code>ICallBack</code> 类的 <a href="#onchannelattrupdated-android"><span>onChannelAttrUpdated</span></a> 回调。</td>
</tr>
<tr><td><a href="#messagechannelsend-android"><span>messageChannelSend()</span></a></td>
<td><p>发送频道消息（消息发送者必须在频道内）</p>
<ul>
<li>成功： 自己收到 <code>ICallBack</code> 类的 <a href="#onmessagesendsuccess-android"><span>onMessageSendSuccess</span></a> 回调，频道内所有用户收到 <code>ICallback</code> 类的 <a href="#onmessagechannelreceive-android"><span>onMessageChannelReceive</span></a> 回调；</li>
<li>失败： 自己收到 <code>ICallBack</code> 类的 <a href="#onmessagesenderror-android"><span>onMessageSendError</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#messagechannelsendforce-android"><span>messageChannelSendForce()</span></a></td>
<td>发送频道消息（消息发送者不必在频道内）</td>
</tr>
<tr><td><a href="#channelinviteuser-android"><span>channelInviteUser()</span></a></td>
<td><p>邀请名为 account 的用户加入指定频道</p>
<ul>
<li>成功： 自己收到 <code>ICallBack</code> 类的 <a href="#oninviteacceptedbypeer-android"><span>onInviteAcceptedByPeer</span></a> 回调， 受邀用户收到 <code>ICallback</code> 类的 <a href="#callbackset-android"><span>onInviteReceived</span></a> 回调；</li>
<li>失败： 自己收到 <code>ICallBack</code> 类的 <a href="#oninvitefailed-android"><span>onInviteFailed</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#channelinviteuser2-android"><span>channelInviteUser2()</span></a></td>
<td><p>邀请名为 account 的用户加入指定频道，呼叫方可以附带一段额外信息。</p>
<ul>
<li>成功： 自己收到 <code>ICallBack</code> 类的 <a href="#oninviteacceptedbypeer-android"><span>onInviteAcceptedByPeer</span></a> 回调， 受邀用户收到 <code>ICallback</code> 类的 <a href="#callbackset-android"><span>onInviteReceived</span></a> 回调；</li>
<li>失败： 自己收到 <code>ICallBack</code> 类的 <a href="#oninvitefailed-android"><span>onInviteFailed</span></a> 回调。</li>
</ul>
</td>
</tr>
<tr><td><a href="#channelinvitedtmf-android"><span>channelInviteDTMF()</span></a></td>
<td>发送 DTMF 消息到对端 <code>phoneNum</code> 用户，一般用于 SIP 网关的呼叫。</td>
</tr>
<tr><td><a href="#channelinviteaccept-android"><span>channelInviteAccept()</span></a></td>
<td>接受来自 account 用户的加入指定频道的呼叫邀请。接收后主叫方将收到 <a href="#oninviteacceptedbypeer-android"><span>onInviteAcceptedByPeer</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinviterefuse-android"><span>channelInviteRefuse()</span></a></td>
<td>拒绝来自 account 用户的加入指定频道的呼叫邀请。拒绝后主叫方将收到 <a href="#oninviterefusedbypeer-android"><span>onInviteRefusedByPeer</span></a> 回调。</td>
</tr>
<tr><td><a href="#channelinviteend-android"><span>channelInviteEnd()</span></a></td>
<td>终止向 account 用户发送加入指定频道的邀请。终止成功后主叫方将收到 <a href="#oninviteendbymyself-android"><span>onInviteEndByMyself</span></a> 回调。</td>
</tr>
<tr><td><a href="#getstatus-android"><span>getStatus()</span></a></td>
<td>获取用户登录状态 （未登录、正在登录、登录成功、正在重连）</td>
</tr>
<tr><td><a href="#getsdkversion-android"><span>getSdkVersion()</span></a></td>
<td>获取SDK版本，版本号示例：1010104019</td>
</tr>
</tbody>
</table>



### 生命周期

#### <a name="getinstance-android"></a>获取 AgoraAPIOnlySignal 实例 \(getInstance\)

该方法用于获取 <code>AgoraAPIOnlySignal</code> 实例。

```
public static AgoraAPIOnlySignal getInstance(Context context, String appId);
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
<tr><td><code>context</code></td>
<td>Android activity 的 Context（上下文环境）</td>
</tr>
<tr><td><code>appId</code></td>
<td>Agora 提供的 App 账号，详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>App ID</span></a>。</td>
</tr>
</tbody>
</table>

#### <a name="createinstance-android"></a>创建多个 AgoraAPIOnlySignal 实例 \(createAgoraSDKInstance\)

多次调用该方法可创建多个 <code>AgoraAPIOnlySignal</code> 实例。

返回：一个 <code>AgoraAPIOnlySignal</code> 实例。

```
public static AgoraAPIOnlySignal createAgoraSDKInstance(Context context,String appId)
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
<tr><td><code>context</code></td>
<td>Android activity 的 Context（上下文环境）</td>
</tr>
<tr><td><code>appId</code></td>
<td>Agora 提供的 App 账号，详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>App ID</span></a>。</td>
</tbody>
</table>

#### <a name="callbackset-android"></a>设置回调 \(callbackSet\)

该方法用于设置回调。

```
public native void callbackSet(ICallBack handler);
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
<tr><td><code>handler</code></td>
<td><code>ICallBack</code>的<code>handler</code></td>
</tr>
</tbody>
</table>



> 使用回调时请避免同步或阻塞操作，否则可能导致系统崩溃。

#### <a name="callbackget-android"></a>获取回调 \(callbackGet\)

该方法用于获取 <code>ICallBack</code> 对象。

```
public ICallBack callbackGet();
```

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>返回值</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>ICallBack</code></td>
<td><code>ICallBack</code>对象</td>
</tr>
</tbody>
</table>



#### <a name="destroy-android"></a>销毁信令实例 \(destroy\)

该方法用于销毁信令实例。

```
public void destroy();
```

### 用户系统

#### <a name="login-android"></a>登录 \(login\)

```
public void login(String appId,String account,String token,int uid,String deviceID);
```

该方法用于登录 Agora 信令系统。用户在进行任何操作前，必须先登录。

- 登录成功：回调 <code>onLoginSuccess</code>，
- 登录失败：回调 <code>onLoginFailed</code>，
- 登录之后失去与服务器的连接：回调 <code>onLogout</code>。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>appId</td>
<td>Agora 提供的 App ID，详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>App ID</span></a>。</td>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号，最大 128 字节可见字符（不能使用空格）。可以是用户的 uid、昵称、guid 等任何内容，但必须保证唯一。本文提到的所有 <code>account</code> 参数都是如此。</td>
</tr>
<tr><td><code>token</code></td>
<td>由 App ID 和 App 证书生成的 SignalingToken，详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>密钥说明</span></a></td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>deviceID</code></td>
<td>暂时无用，设置为空</td>
</tr>
</tbody>
</table>



> 在测试环境下您可以将参数 token 设为 <code>_no_need\_token</code> 表示不使用秘钥，但是声网不建议在生产环境下不使用动态秘钥。 默认情况下如果当前已经处于登录状态，调用 <code>login</code> 方法会被忽略。如果希望踢掉老的登录，可以在 <code>login</code> 之前调用 <code>logout</code> 且不用等退出成功就可登录。

#### <a name="login2-android"></a>登录 \(login2\)

```
public void login2(String appId,String account,String token,int uid,String deviceID,int retry_time_in_s, int retry_count);
```

该方法用于登录 Agora 信令系统。用户在进行任何操作前，必须先登录。

- 登录成功：回调 <code>onLoginSuccess</code>，
- 登录失败：回调 <code>onLoginFailed</code>，
- 登录之后失去与服务器的连接：回调 <code>onLogout</code>。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>appId</td>
<td>App ID, 详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>App ID</span></a>。</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号，最大 128 字节可见字符（不能使用空格）。可以是用户的 uid、昵称、guid 等任何内容，但必须保证唯一。本文提到的所有 <code>account</code> 参数都是如此。</td>
</tr>
<tr><td><code>token</code></td>
<td>由 App ID 和 App 证书生成的 SignalingToken ，详见 <a href="../../cn/Agora%20Platform/key_signaling.md"><span>SignalingToken</span></a>。</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>deviceID</code></td>
<td>暂时无用，设置为空</td>
</tr>
<tr><td><code>retry_time_in_s</code></td>
<td>登录重试时间，默认为 30 秒</td>
</tr>
<tr><td><code>retry_count</code></td>
<td>登录重试次数，默认为 3 次</td>
</tr>
</tbody>
</table>



> 在测试环境下您可以将参数 token 设为 `_no_need_token` 表示不使用秘钥，但是声网不建议在生产环境下不使用动态秘钥。 当达到 `retry_time_in_s` 或 `retry_count` 其中任一上限时，将触发 `onLoginFailed` 回调并停止重连。

#### <a name="logout-android"></a>登出 \(logout\)

```
public void logout();
```

该方法允许用户退出 Agora 信令系统。成功退出 Agora 信令系统时会触发 <code>onLogout</code> 回调。

#### <a name="invoke-android"></a>RPC 远程过程调用 \(invoke\)

```
public void invoke(String name, String req, String callID);
```

该方法用于远程过程调用。调用成功将触发 <code>onInvokeRet</code> 回调。

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
<li>name: io.agora.signal.user_query_user_status</li>
<li>req: {“account”:用户名}</li>
<li>返回值：status(1:在线；0:不在线)</li>
</ul>
</td>
</tr>
<tr><td>设置用户属性</td>
<td><ul>
<li>name: io.agora.signal.user_set_attr</li>
<li>req: {“name”:属性名,”value”:属性值}</li>
</ul>
</td>
</tr>
<tr><td>获取用户属性</td>
<td><ul>
<li>name: io.agora.signal.user_get_attr</li>
<li>req: {“account”:用户名,”name”:属性名}</li>
<li>返回值: {“value”:属性值}</li>
</ul>
</td>
</tr>
<tr><td>获取用户所有属性</td>
<td><ul>
<li>name: io.agora.signal.user_get_attr_all</li>
<li>req: {“account”:用户名}</li>
<li>返回值：所有属性的 JSON 值</li>
</ul>
</td>
</tr>
<tr><td>查询频道人数</td>
<td><ul>
<li>name: io.agora.signal.channel_query_num</li>
<li>req: {“name”:频道名}</li>
<li>返回值：总人数</li>
</ul>
</td>
</tr>
<tr><td>查询是否在频道内</td>
<td><ul>
<li>name: io.agora.signal.channel_query_user_isin</li>
<li>req: {“name”:频道名,”account”:用户名}</li>
<li>返回值：isin（1: 在频道内；0:不在频道内）</li>
</ul>
</td>
</tr>
<tr><td>查询频道用户列表</td>
<td><ul>
<li>name: io.agora.signal.channel_query_userlist</li>
<li>req: {“name”:频道名}</li>
<li>返回值：{“num”:总人数,”list”:最近成员}</li>
</ul>
</td>
</tr>
<tr><td>查询频道最近用户列表</td>
<td><b>已废弃</b><ul>
<li>name: io.agora.signal.channel_query_userlist_all</li>
<li>req: {“name”:频道名,”num”:数量(默认 1000*1000)}</li>
<li>返回值：{“num”:总人数,”list”:最近成员}</li>
<li>建议服务端使用，客户端使用注意频率限制</li>
</ul>
</td>
</tr>
<tr><td>清空频道属性</td>
<td><ul>
<li>name: io.agora.signal.channel_clear_attr</li>
<li>req: {“channel”:频道名}</li>
</ul>
</td>
</tr>
<tr><td>删除频道属性</td>
<td><ul>
<li>name: io.agora.signal.channel_del_attr</li>
<li>req: {“channel”:频道名,”name”:属性名}</li>
</ul>
</td>
</tr>
<tr><td>设置频道属性</td>
<td><ul>
<li>name: io.agora.signal.channel_set_attr</li>
<li>req: {“channel”:频道名,”name”:属性名,”value”:属性值}</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### <a name="queryuserstatus-android"></a>查询用户状态 \(queryUserStatus\)

```
public void queryUserStatus(String account);
```

该方法用于查询用户是否在线。

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
</tbody>
</table>



#### <a name="setattr-android"></a>设置用户属性 \(setAttr\)

```
public void setAttr(String name,String value);
```

该方法用于设置用户属性。

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
<td>属性名称。最大 128 字节可见字符，</td>
</tr>
<tr><td><code>value</code></td>
<td>属性值。最大 8096 字节可见字符</td>
</tr>
</tbody>
</table>



#### <a name="getattr-android"></a>获取自己的属性 (getAttr)

该方法用于获取自己的用户属性。调用成功会回调 <code>onUserAttrResult</code>。

```
public void getAttr(String name);
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
<td>属性名称。最大 128 字节可见字符</td>
</tr>
</tbody>
</table>



#### <a name="getattrall-android"></a>获取自己的全部属性 \(getAttrAll\)

```
public void getAttrAll();
```

该方法用于获取自己的全部用户属性。调用成功会回调 <code>onUserAttrAllResult</code>。

#### <a name="getuserattr-android"></a>获取某个用户的属性 \(getUserAttr\)

```
public void getUserAttr(String account,String name);
```

该方法用于获取某个特定用户的用户属性。调用成功会回调 <code>onUserAttrResult</code>。

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
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>name</code></td>
<td>属性名称。最大 128 字节可见字符</td>
</tr>
</tbody>
</table>



#### <a name="getuserattrall-android"></a>获取某个用户的所有属性 \(getUserAttrAll\)

```
public void getUserAttrAll(String account);
```

该方法用于获取某个用户的所有用户属性。调用成功会回调 <code>onUserAttrAllResult</code>。

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
<td>客户端定义的用户账号</td>
</tr>
</tbody>
</table>



#### <a name="messageinstantsend-android"></a>发送点对点消息 \(messageInstantSend\)

```
public void messageInstantSend(String account,int uid,String msg,String msgID);
```

该方法发送点对点消息到某个指定账号。

- 发送成功本地将回调 <code>onMessageSendSuccess</code>，对方将收到 <code>onMessageInstantReceive</code> 回调。
- 发送失败将回调 <code>onMessageSendError</code> 。

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
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文。每条消息最大为 8196 字节可见字符</td>
</tr>
<tr><td><code>msgID</code></td>
<td>消息的 ID。声网建议将其设置为”“（即由 SDK 接管 msgID 的生成、分配并保证唯一）</td>
</tr>
</tbody>
</table>



### 频道系统

#### <a name="channeljoin-android"></a>加入频道 \(channelJoin\)

```
public void channelJoin(String channelID);
```

该方法让用户加入指定频道。用户一次只能加入一个频道。

用户加入频道成功后，自己将收到回调 <code>onChannelJoined</code>，其他同一频道内用户将收到回调 <code>onChannelUserJoined</code>。用户加入失败后，自己将收到回调 <code>onChannelJoinFailed</code>。

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
<td><p>频道名。最大为 128 字节可见字符。包含以下特殊频道名（已废弃，不建议使用）：</p>
<div><ul>
<li><code>__agora_user_online</code> ：当前 <code>appId</code> 中所有用户登录或离线事件将发送至该频道。</li>
<li><code>__agora_channel_event</code> : 当前 <code>appId</code> 中用户加入或离开频道的所有事件将发送至该频道。</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



> 特殊频道名供服务端使用，加入频道即可获取相关事件，如登录事件、频道事件。

#### <a name="channelleave-android"></a>离开频道 \(channelLeave\)

```
public void channelLeave(String channelID);
```

该方法让用户退出当前频道。退出成功后，所有频道用户将收到回调 <code>onChannelUserLeaved</code>。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
</tbody>
</table>



#### <a name="channelqueryusernum-android"></a>查询频道用户数 \(channelQueryUserNum\)

```
public void channelQueryUserNum(String channelID);
```

该方法用于查询指定频道的用户数量。当操作成功，用户将收到 <code>onChannelQueryUserNumResult</code> 回调。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
</tbody>
</table>



#### <a name="channelsetattr-android"></a>设置频道属性 \(channelSetAttr\)

```
public void channelSetAttr(String channelID,String name,String value);
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
<tr><td><code>channelID</code></td>
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>name</code></td>
<td>属性名称。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>value</code></td>
<td>属性值。最大为 8096 字节可见字符</td>
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

#### <a name="channeldelattr-android"></a>删除频道属性 \(channelDelAttr\)

```
public void channelDelAttr(String channelID,String name);
```

该方法用于删除频道属性。当操作成功，所有频道用户将收到 <code>onChannelAttrUpdated</code> 回调。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>name</code></td>
<td>属性名称。最大为 128 字节可见字符</td>
</tr>
</tbody>
</table>



#### <a name="channelclearattr-android"></a>删除所有频道属性 \(channelClearAttr\)

```
public void channelClearAttr(String channelID);
```

该方法用于删除所有频道属性。当操作成功，所有频道用户都将收到 <code>onChannelAttrUpdated</code> 回调。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
</tbody>
</table>



#### <a name="messagechannelsend-android"></a>发送频道消息 \(messageChannelSend\)

```
public void messageChannelSend(String channelID,String msg,String msgID);
```

该方法用于发送频道消息，频道内所有成员会收到 <code>onMessageChannelReceive</code> 回调。

发送成功会回调 <code>onMessageSendSuccess</code>，发送失败会回调 <code>onMessageSendError</code>。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文。每条频道消息最大为 8K 字节可见字符。每个用户每秒不能发超过 60 条消息，整个频道每秒不能发超过 200 条消息。</td>
</tr>
<tr><td><code>msgID</code></td>
<td>可见字符，消息的 ID。声网建议将其设置为”“（即由 SDK 接管 msgID 的生成、分配并保证唯一）</td>
</tr>
</tbody>
</table>



#### <a name="messagechannelsendforce-android"></a>发送频道消息 \(messageChannelSendForce\)

```
public native void messageChannelSendForce(String channelID,String msg,String msgID);
```

该方法用于在不加入频道的情况下发送频道消息。建议不要在应用程序端使用该方法。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>channelID</td>
<td>频道 ID</td>
</tr>
<tr><td><code>msg</code></td>
<td>待发送的消息</td>
</tr>
<tr><td><code>msgID</code></td>
<td>消息的 ID。声网建议将其设置为”“（即由 SDK 接管 <code>msgID</code> 的生成、分配并保证唯一）</td>
</tr>
</tbody>
</table>



### 呼叫系统

#### <a name="channelinviteuser-android"></a>发起呼叫 \(channelInviteUser\)

```
public void channelInviteUser(String channelID,String account,int uid);
```

该方法用于发起呼叫，即邀请某用户加入某个频道。

> 呼叫和加入频道是两个独立的过程。用户必须自己再另行加入频道，用户可以选择：先加入频道，再发送呼叫邀请或先发送呼叫邀请，对方接受后再加入频道。

如果呼叫失败，会回调 <code>onInviteFailed</code>。可能的原因有：

- 对方不在线
- 本端网络不通
- 服务器异常

如果收到对方的确认信息，本地将回调 <code>onInviteReceivedByPeer</code>, 对方会回调 <code>onInviteReceived</code>。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
</tbody>
</table>



#### <a name="channelinviteuser2-android"></a>发起呼叫 \(channelInviteUser2\)

```
public void channelInviteUser2(String channelID,String account,String extra);
```

与 <code>channelInviteUser</code> 功能相同，参数不同。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>extra</code></td>
<td><p>主叫方想传递给呼叫方的其他信息，最大为 8K 字节可见字符。必须为 JSON 格式。如：</p>
<ul>
<li>{“_require_peer_online”:1}  如果对方不在线，则立即触发 <code>ICallBack</code> 回调接口类的 <code>onInviteFailed</code> 回调</li>
<li>{“_require_peer_online”:0}  如果对方不在线超过 20 秒，则触发 <code>ICallBack</code> 回调接口类的 <code>onInviteFailed</code> 回调（默认）</li>
<li>{“_timeout”:30} 呼叫超时。超时后收到 <code>inviteFailed</code> 的同时消息被销毁，被叫不再收到。范围：0～30s。</li>
<li>{“destMediaUid” : 123}  指定的加入相应媒体频道的 <code>uid</code>。</li>
<li>{“srcNum” : “+123456789”} 指定的在远端手机屏幕上显示的手机号码。</li>
</ul>
</td>
</tr>
</tbody>
</table>



> SIP 呼叫时你不需要设置 `_require_peer_online` 字段。

#### <a name="channelinvitedtmf-android"></a>向远端发送 DTMF 消息 \(channelInviteDTMF\)

```
public void channelInviteDTMF(String channelID,String phoneNum,String dtmf);
```

该方法发送 DTMF 消息到对端，用于 SIP 网关的呼叫。对方会回调 <code>onInviteMsg</code> 。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>phoneNum</code></td>
<td>对方的电话号码</td>
</tr>
<tr><td><code>dtmf</code></td>
<td>拨通后，后续需要输入的模拟电话按键 (DTMF)</td>
</tr>
</tbody>
</table>



#### <a name="channelinviteaccept-android"></a>接受呼叫 \(channelInviteAccept\)

```
public void channelInviteAccept(String channelID,String account,int uid,String extra);
```

该方法用于当收到呼叫时，调用本接口接受收到的呼叫。主叫方将回调 <code>onInviteAcceptedByPeer</code> 。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>extra</code></td>
<td>JSON 字符串或为”“。用户自定义字段，可用于 SIP 呼叫。</td>
</tr>
</tbody>
</table>



#### <a name="channelinviterefuse-android"></a>拒绝呼叫 \(channelInviteRefuse\)

```
public void channelInviteRefuse(String channelID,String account,int uid, String extra);
```

该方法用于当收到呼叫时，调用本接口拒绝收到的呼叫。对方会收到 <code>onInviteRefusedByPeer</code> 的回调。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>extra</code></td>
<td>其他信息。最大为 8K 字节可见字符。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



#### <a name="channelinviteend-android"></a>结束呼叫 (channelInviteEnd)

```
public void channelInviteEnd(String channelID,String account,int uid);
```

该方法用于当呼叫接通后，调用本接口关闭呼叫。本端会回调 <code>onInviteEndByMyself</code>，对端会回调 <code>onInviteEndByPeer</code>。

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
<td>频道名。最大为 128 字节可见字符</td>
</tr>
<tr><td><code>account<code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
</tbody>
</table>



### 其他方法

#### <a name="getstatus-android"></a>获取用户状态 \(getStatus\)

```
public int getStatus();
```

该方法用于获取用户状态信息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>返回值</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>LOGIN_STATE_NOT_LOGIN</td>
<td>0: 用户未登录。</td>
</tr>
<tr><td>LOGIN_STATE_LOGINING</td>
<td>1: 用户正在登录。</td>
</tr>
<tr><td>LOGIN_STATE_LOGINED</td>
<td>2: 用户已登录。</td>
</tr>
<tr><td>LOGIN_STATE_RECONNECTING</td>
<td>3: 用户正在重建连接。</td>
</tr>
</tbody>
</table>



#### <a name="getsdkversion-android"></a>获取 SDK 版本 (getSdkVersion)

```
public int getSdkVersion();


```

该方法用于获取 SDK 版本信息。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>返回值</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>SDK 版本号</td>
<td>如返回 1010104019 则表示版本号为 1.1.4.19.</td>
</tr>
</tbody>
</table>



## ICallBack 回调接口类

ICallBack 类为信令的回调接口类。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>回调</strong></td>
<td><strong>说明</strong></td>
</tr>
<tr><td><a href="#onreconnecting-android"><span>onReconnecting()</span></a></td>
<td>连接丢失回调</td>
</tr>
<tr><td><a href="#onerror-android"><span>onError()</span></a></td>
<td>出错回调</td>
</tr>
<tr><td><a href="#onqueryuserstatusresult-android"><span>onQueryUserStatusResult()</span></a></td>
<td>用户状态查询回调</td>
</tr>
<tr><td><a href="#onreconnected-android"><span>onReconnected()</span></a></td>
<td>重连成功回调</td>
</tr>
<tr><td><a href="#onloginsuccess-android"><span>onLoginSuccess()</span></a></td>
<td>登录成功回调</td>
</tr>
<tr><td><a href="#onlogout-android"><span>onLogout()</span></a></td>
<td>退出登录回调</td>
</tr>
<tr><td><a href="#onloginfailed-android"><span>onLoginFailed()</span></a></td>
<td>登录失败回调</td>
</tr>
<tr><td><a href="#oninvokeret-android"><span>onInvokeRet()</span></a></td>
<td>RPC 远程过程调用成功回调</td>
</tr>
<tr><td><a href="#onchanneljoined-android"><span>onChannelJoined()</span></a></td>
<td>加入频道回调</td>
</tr>
<tr><td><a href="#onchanneljoinfailed-android"><span>onChannelJoinFailed()</span></a></td>
<td>加入频道失败回调</td>
</tr>
<tr><td><a href="#onchannelleaved-android"><span>onChannelLeaved()</span></a></td>
<td>离开频道回调</td>
</tr>
<tr><td><a href="#onchanneluserjoined-android"><span>onChannelUserJoined()</span></a></td>
<td>其他用户加入频道回调</td>
</tr>
<tr><td><a href="#onchanneluserleaved-android"><span>onChannelUserLeaved()</span></a></td>
<td>其他用户离开频道回调</td>
</tr>
<tr><td><a href="#onchanneluserlist-android"><span>onChannelUserList()</span></a></td>
<td>获取频道内用户列表回调</td>
</tr>
<tr><td><a href="#onchannelqueryusernumresult-android"><span>onChannelQueryUserNumResult()</span></a></td>
<td>返回查询的用户数量回调</td>
</tr>
<tr><td><a href="#onchannelattrupdated-android"><span>onChannelAttrUpdated()</span></a></td>
<td>频道属性发生变化回调</td>
</tr>
<tr><td><a href="#oninvitereceived-android"><span>onInviteReceived()</span></a></td>
<td>收到呼叫邀请回调</td>
</tr>
<tr><td><a href="#oninvitereceivedbypeer-android"><span>onInviteReceivedByPeer()</span></a></td>
<td>远端已收到呼叫回调</td>
</tr>
<tr><td><a href="#oninviteacceptedbypeer-android"><span>onInviteAcceptedByPeer()</span></a></td>
<td>远端已接受呼叫回调</td>
</tr>
<tr><td><a href="#oninviterefusedbypeer-android"><span>onInviteRefusedByPeer()</span></a></td>
<td>对方已拒绝呼叫回调</td>
</tr>
<tr><td><a href="#oninvitefailed-android"><span>onInviteFailed()</span></a></td>
<td>呼叫失败回调</td>
</tr>
<tr><td><a href="#oninviteendbypeer-android"><span>onInviteEndByPeer()</span></a></td>
<td>对方已结束呼叫回调</td>
</tr>
<tr><td><a href="#oninviteendbymyself-android"><span>onInviteEndByMyself()</span></a></td>
<td>本地已结束呼叫回调</td>
</tr>
<tr><td><a href="#oninvitemsg-android"><span>onInviteMsg()</span></a></td>
<td>本地已收到消息回调</td>
</tr>
<tr><td><a href="#onmessagesenderror-android"><span>onMessageSendError()</span></a></td>
<td>消息发送失败回调</td>
</tr>
<tr><td><a href="#onmessagesendsuccess-android"><span>onMessageSendSuccess()</span></a></td>
<td>消息已发送成功回调</td>
</tr>
<tr><td><a href="#onmessageinstantreceive-android"><span>onMessageInstantReceive()</span></a></td>
<td>接收方收到消息后，接收方收到的回调</td>
</tr>
<tr><td><a href="#onmessagechannelreceive-android"><span>onMessageChannelReceive()</span></a></td>
<td>收到频道消息回调</td>
</tr>
<tr><td><a href="#onlog-android"><span>onLog()</span></a></td>
<td>已打印日志回调</td>
</tr>
<tr><td><a href="#onuserattrresult-android"><span>onUserAttrResult()</span></a></td>
<td>已获取用户属性查询结果回调</td>
</tr>
<tr><td><a href="#onuserattrallresult-android"><span>onUserAttrAllResult()</span></a></td>
<td>已获取所有用户属性查询结果回调</td>
</tr>
</tbody>
</table>



### 回调事件

#### <a name="onreconnecting-android"></a>连接丢失回调 \(onReconnecting\)

```
public void onReconnecting(int nretry);


```

与Agora信令系统的连接时触发本回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>nretry</code></td>
<td>当前重连的次数</td>
</tr>
</tbody>
</table>



#### <a name="onerror-android"></a>出错回调 \(onError\)

```
public void onError(String name,int ecode,String desc)


```

该回调返回详细的出错信息。

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
<td>错误类型</td>
</tr>
<tr><td><code>ecode</code></td>
<td><p>错误码。</p>
<ul>
<li>GENERAL_E = 1000 : 一般错误</li>
<li>GENERAL_E_FAILED = 1001: 操作失败</li>
<li>GENERAL_E_UNKNOWN = 1002: 一般错误 - 未知</li>
<li>GENERAL_E_NOT_LOGIN = 1003: 未登录</li>
<li>GENERAL_E_WRONG_PARAM = 1004: 参数错误</li>
<li>GENERAL_E_LARGE_PARAM = 1005: 参数过大</li>
</ul>
</td>
</tr>
<tr><td><code>desc</code></td>
<td>错误的内容</td>
</tr>
</tbody>
</table>



#### <a name="onqueryuserstatusresult-android"></a>用户状态查询回调 \(onQueryUserStatusResult\)

```
public void onQueryUserStatusResult(String name,String status)


```

该回调返回用户状态查询结果。调用 <code>queryUserStatus</code> 方法时触发此回调。

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
<td>用户账号名称</td>
</tr>
<tr><td><code>status</code></td>
<td><p>用户状态：是否在线</p>
<ul>
<li>1: 用户在线</li>
<li>0: 用户离线</li>
</ul>
</td>
</tr>
</tbody>
</table>



#### <a name="onreconnected-android"></a>重连成功回调 \(onReconnected\)

```
public void onReconnected(int fd);


```

- 当与 Agora 信令系统重连成功时会触发此回调。
- 重连失败会触发 <code>onLogout</code> 回调。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>fd</code></td>
<td>仅供 Agora 内部使用</td>
</tr>
</tbody>
</table>



#### <a name="onloginsuccess-android"></a>登录成功回调 \(onLoginSuccess\)

```
public void onLoginSuccess(int uid,int fd);


```

当登录成功时触发此回调。

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
<tr><td><code>fd</code></td>
<td>仅供 Agora 内部使用</td>
</tr>
</tbody>
</table>



#### <a name="onlogout-android"></a>退出登录回调 \(onLogout\)

```
public void onLogout(int ecode);


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
<tr><td><code>ecode</code></td>
<td>错误码</td>
</tr>
</tbody>
</table>



#### <a name="onloginfailed-android"></a>登录失败回调 \(onLoginFailed\)

```
public void onLoginFailed(int ecode);


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
<td>错误码</td>
</tr>
</tbody>
</table>



#### <a name="oninvokeret-android"></a>RPC 远程过程调用成功回调 \(onInvokeRet\)

```
void onInvokeRet(String callID, String err, String resp);


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
<tr><td><code>callID</code></td>
<td>由 invoke传入的 <code>callID</code></td>
</tr>
<tr><td><code>err</code></td>
<td>错误码</td>
</tr>
<tr><td><code>resp</code></td>
<td>返回的 JSON 值</td>
</tr>
</tbody>
</table>



当 RPC 远程过程调用成功触发此回调。

#### <a name="onchanneljoined-android"></a>加入频道回调 \(onChannelJoined\)

```
public void onChannelJoined(String channelID);


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
<tr><td><code>channelID</code></td>
<td>频道名。最大为 128 字节可见字符</td>
</tr>
</tbody>
</table>



#### <a name="onchanneljoinfailed-android"></a>加入频道失败回调 \(onChannelJoinFailed\)

```
public void onChannelJoinFailed(String channelID,int ecode);


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
<tr><td><code>channelID</code></td>
<td>频道名</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误码</td>
</tr>
</tbody>
</table>



#### <a name="onchannelleaved-android"></a>离开频道回调 \(onChannelLeaved\)

```
public void onChannelLeaved(String channelID,int ecode);


```

当离开频道成功时触发此回调。

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
<tr><td><code>ecode</code></td>
<td>错误码</td>
</tr>
</tbody>
</table>



#### <a name="onchanneluserjoined-android"></a>其他用户加入频道回调 \(onChannelUserJoined\)

```
public void onChannelUserJoined(String account,int uid);


```

当有用户加入频道时触发此回调。

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
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
</tbody>
</table>



#### <a name="onchanneluserleaved-android"></a>其他用户离开频道回调 \(onChannelUserLeaved\)

```
public void onChannelUserLeaved(String account,int uid);


```

当有用户离开频道触发此回调。

#### <a name="onchanneluserlist-android"></a>获取频道内用户列表回调 \(onChannelUserList\)

```
public void onChannelUserList(String[] accounts, int[] uids);


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
<tr><td><code>accounts</code></td>
<td>用户账号列表</td>
</tr>
<tr><td><code>uids</code></td>
<td>频道内用户列表</td>
</tr>
</tbody>
</table>



#### <a name="onchannelqueryusernumresult-android"></a>返回查询的用户数量回调 \(onChannelQueryUserNumResult\)

```
public void onChannelQueryUserNumResult(String channelID,int ecode,int num);


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
<tr><td><code>channelID</code></td>
<td>频道名</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误码</td>
</tr>
<tr><td><code>num</code></td>
<td>查询结果</td>
</tr>
</tbody>
</table>



#### <a name="onchannelattrupdated-android"></a>频道属性发生变化回调 (onChannelAttrUpdated)

```
public void onChannelAttrUpdated(String channelID,String name,String value,String type);


```

当频道属性变化时触发此回调。

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
<tr><td><code>name</code></td>
<td>属性名</td>
</tr>
<tr><td><code>value</code></td>
<td>属性值</td>
</tr>
<tr><td><code>type</code></td>
<td><p>变化类型:</p>
<div><ul>
<li>“update”：更新</li>
<li>“del”：删除</li>
<li>“clear”：全部删除</li>
<li>“set”: 废弃类型</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



#### <a name="oninvitereceived-android"></a>收到呼叫邀请回调 \(onInviteReceived\)

```
  public void onInviteReceived(String channelID,String account,int uid,String extra);

当收到呼叫邀请时触发此回调。


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
<tr><td><code>channelID</code></td>
<td>频道名</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>extra</code></td>
<td>呼叫方向对方发送的其他信息，最大为 8K 字节可见字符。必须为 JSON 模式。</td>
</tr>
</tbody>
</table>



#### <a name="oninvitereceivedbypeer-android"></a>远端已收到呼叫回调 \(onInviteReceivedByPeer\)

```
public void onInviteReceivedByPeer(String channelID,String account,int uid);


```

当呼叫被对方收到时触发。

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
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
</tbody>
</table>



#### <a name="oninviteacceptedbypeer-android"></a>远端已接受呼叫回调 \(onInviteAcceptedByPeer\)

```
public void onInviteAcceptedByPeer(String channelID,String account,int uid, String extra);


```

当呼叫被对方接受时触发此回调。

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
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>extra</code></td>
<td>呼叫方向对方发送的其他信息，如该呼叫为视频通话还是语音通话。必须为 JSON 模式。</td>
</tr>
</tbody>
</table>



#### <a name="oninviterefusedbypeer-android"></a>对方已拒绝呼叫回调 \(onInviteRefusedByPeer\)

```
public void onInviteRefusedByPeer(String channelID,String account,int uid, String extra);


```

当呼叫被对方拒绝时触发此回调。

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
<td>频道名</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>extra</code></td>
<td>呼叫方向对方发送的其他信息，例如：该呼叫为视频通话还是语音通话。必须为 JSON 格式。</td>
</tr>
</tbody>
</table>



#### <a name="oninvitefailed-android"></a>呼叫失败回调 \(onInviteFailed\)

```
public void onInviteFailed(String channelID,String account,int uid,int ecode, String extra);


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
<tr><td><code>channelID</code></td>
<td>频道名</td>
</tr>
<tr><td><code>account</code></td>
<td>被呼叫用户的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误码</td>
</tr>
<tr><td><code>extra</code></td>
<td>关于该呼叫的其他信息, 可以是一个 JSON 字符串或将其设置为空</td>
</tr>
</tbody>
</table>



#### <a name="oninviteendbypeer-android"></a>对方已结束呼叫回调 \(onInviteEndByPeer\)

```
public void onInviteEndByPeer(String channelID,String account,int uid, String extra);


```

当呼叫被对方结束时触发此回调。

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
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>extra</code></td>
<td>呼叫方需传送的其他信息, 如该呼叫为视频通话还是语音通话。可以是一个 JSON 字符串或将其设置为空</td>
</tr>
</tbody>
</table>



#### <a name="oninviteendbymyself-android"></a>本地已结束呼叫回调 \(onInviteEndByMyself\)

```
public void onInviteEndByMyself(String channelID,String account,int uid);


```

当呼叫被自己结束时触发。

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
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
</tbody>
</table>



#### <a name="oninvitemsg-android"></a>本地已收到消息回调 \(onInviteMsg\)

```
public void onInviteMsg(String channelID, String account, int uid, String msgType, String msgData, String extra)


```

当本地接收到远端用户发送的 DTMF 消息时触发该回调。

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
<td>频道名。最大 128 字节可见字符</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>msgType</code></td>
<td>消息类型。目前指的是 DTMF</td>
</tr>
<tr><td><code>msgData</code></td>
<td>DTMF 消息内容。支持的字符串范围为: 0-9, * , #</td>
</tr>
<tr><td><code>extra</code></td>
<td>关于该呼叫的其他信息, 可以是一个 JSON 字符串或将其设置为空</td>
</tr>
</tbody>
</table>



#### <a name="onmessagesenderror-android"></a>消息发送失败回调 \(onMessageSendError\)

```
public void onMessageSendError(String messageID,int ecode);


```

当发送消息失败时触发。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>messageID</code></td>
<td>消息 ID</td>
</tr>
<tr><td><code>ecode</code></td>
<td>错误码</td>
</tr>
</tbody>
</table>



#### <a name="onmessagesendsuccess-android"></a>消息已发送成功回调 \(onMessageSendSuccess\)

```
public void onMessageSendSuccess(String messageID);


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
<tr><td><code>messageID</code></td>
<td>消息 ID</td>
</tr>
</tbody>
</table>



#### <a name="onmessageinstantreceive-android"></a>接收方收到消息时接收方收到的回调 \(onMessageInstantReceive\)

```
public void onMessageInstantReceive(String account,int uid,String msg);


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
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文</td>
</tr>
</tbody>
</table>



#### <a name="onmessagechannelreceive-android"></a>收到频道消息回调 \(onMessageChannelReceive\)

```
public void onMessageChannelReceive(String channelID,String account,int uid,String msg);


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
<tr><td><code>channelID</code></td>
<td>频道名</td>
</tr>
<tr><td><code>account</code></td>
<td>客户端定义的用户账号</td>
</tr>
<tr><td><code>uid</code></td>
<td>固定填 0</td>
</tr>
<tr><td><code>msg</code></td>
<td>消息正文</td>
</tr>
</tbody>
</table>



#### <a name="onlog-android"></a>已打印日志回调 \(onLog\)

```
public void onLog(String txt)


```

当有日志打印时触发。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>txt</code></td>
<td>一行日志的内容</td>
</tr>
</tbody>
</table>



#### <a name="onuserattrresult-android"></a>已获取用户属性查询结果回调 \(onUserAttrResult\)

```
public void onUserAttrResult(String account,String name,String value);


```

当用户已查询到某个用户的属性后即触发该回调。

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
<td>用户登录客户端的账号</td>
</tr>
<tr><td><code>name</code></td>
<td>属性名</td>
</tr>
<tr><td><code>value</code></td>
<td>所有属性的 JSON 值</td>
</tr>
</tbody>
</table>



#### <a name="onuserattrallresult-android"></a>已获取所有用户属性查询结果回调 \(onUserAttrAllResult\)

```
public void onUserAttrAllResult(String account,String value);


```

当用户已查询到某个用户的所有属性后即触发该回调。

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
<td>用户登录客户端的账号</td>
</tr>
<tr><td><code>value</code></td>
<td>所有属性的 JSON 值</td>
</tr>
</tbody>
</table>



## 废弃接口

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>接口</strong></td>
<td><strong>原型</strong></td>
</tr>
<tr><td><code>messageDTMFSend</code></td>
<td>(void) messageDTMFSend:(uint32_t)uid msg:(NSString*)msg msgID:(NSString*)msgID;</td>
</tr>
<tr><td><code>channelInvitePhone</code></td>
<td>(void) channelInvitePhone:(NSString*)channelID phoneNum:(NSString*)phoneNum uid:(uint32_t)uid;</td>
</tr>
<tr><td><code>channelInvitePhone2</code></td>
<td>(void) channelInvitePhone2:(NSString*)channelID phoneNum:(NSString*)phoneNum sourcesNum:(NSString*)sourcesNum;</td>
</tr>
<tr><td><code>channelInvitePhone3</code></td>
<td>(void) channelInvitePhone3:(NSString*)channelID phoneNum:(NSString*)phoneNum sourcesNum:(NSString*)sourcesNum extra:(NSString*)extra;</td>
</tr>
<tr><td><code>isOnline</code></td>
<td>(bool) isOnline;</td>
</tr>
</tbody>
</table>



## 错误码和警告码

详见 [错误码和警告码](../../cn/API%20Reference/the_error_signaling.md)。
