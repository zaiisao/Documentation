
---
title: 在服务端生成密钥
description: Guide on how to generate tokens on the server side
platform: All Platforms
updatedAt: Thu Apr 11 2019 06:48:23 GMT+0000 (UTC)
---
# 在服务端生成密钥
本文适用于 2.1 及之后版本的 Agora SDK。通过简单的 API 调用，在服务端生成 Token，在加入频道时使用。

Agora 的鉴权 API 涵盖 Java、Python、CPP、Ruby、Node.js、PHP、Go 语言，你可以根据实际需要，选择相应的语言。

> 声网目前暂不支持用非 0 的 string 型 uid 生成 token。

## Java

### 初始化 Token Builder \(initTokenBuiler\)

```java
public boolean initTokenBuilder(String originToken);
```

该方法初始化 Token Builder。

该方法利用已经存在的 Token，来重新初始化 Token Builder。初始化后，Token Builder 内会包含原来 Token 内的 App ID、App Certificate、 Channel Name、UID 以及 Privilege 信息。

该方法可以结合 [设置用户权限 \(setPrivilege\)](#setPrivilegeJava) 和 [删除用户权限 \(removePrivilege\)](#removePrivilegeJava) ，用于为已经存在的 `originToken` 添加、删除权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>originToken</code></td>
<td>用户的初始 Token</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>

Token Builder 的结构体如下所示：

```java
public SimpleTokenBuilder(String appId, String appCertificate, String channelName, String uid);
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
<tr><td><code>appID</code></td>
<td>Agora 为应用程序开发者签发的 App ID。详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App ID</span></a> 。</td>
</tr>
<tr><td><code>appCertificate</code></td>
<td>Agora 为应用程序开发者签发的 App Certificate。当启用 App Certificate 后，原有的 App ID 会失效，详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App Certificate</span></a>。</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户ID，32位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护</td>
</tr>
</tbody>
</table>



### 初始化用户权限 \(initPrivileges\)

```java
public boolean initPrivileges(Role role);
```

该方法初始化用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>用户角色。根据不同的场景，用户可分为以下角色：</p>
<ul>
<li>Role_Attendee = 0：将用户角色设置为通信场景下参与通话的各方</li>
<li>Role_Publisher = 1：将用户角色设置为直播场景下能发布音视频流的 Publisher</li>
<li>Role_Subscriber = 2：将用户角色设置为直播场景下能订阅音视频流的 Subscriber</li>
<li>Role_Admin = 101：将用户角色设置为通信及直播场景下的管理员</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>

> 用户角色的设置决定了该角色可以拥有哪些权限。关于角色和权限的对应搭配，请见 [设置用户权限 \(setPrivilege\)](#setPrivilege) 。

<a name = "setPrivilegeJava"></a>

### 设置用户权限 \(setPrivilege\)

```java
public void setPrivilege(AccessToken.Privileges privilege, int expireTimestamp) ;
```

该方法设置用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>用户权限，共有4种：</p>
<ul>
<li>AttendeePrivileges：通话方权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
</ul>
</li>
<li>PublisherPrivileges：发布者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kPublishAudiocdn：发布音频 CDN</li>
<li>kPublishVideocdn：发布视频 CDN</li>
<li>kInvitePublishAudioStream：邀请发布音频流 <sup>[1]</sup></li>
<li>kInvitePublishVideoStream：邀请发布视频流 <sup>[1]</sup></li>
<li>kInvitePublishDataStream：邀请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>SubscriberPrivileges：订阅者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kRequestPublishAudioStream：申请发布音频流 <sup>[1]</sup></li>
<li>kRequestPublishVideoStream：申请发布视频流 <sup>[1]</sup></li>
<li>kRequestPublishDataStream：申请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>AdminPrivileges：管理员权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kAdministrateChannel：管理频道</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td><p>服务过期时间：</p>
<div><ul>
<li>0（默认）：服务不过期</li>
<li>&lt; now：拒绝登录，强行下线</li>
<li>&gt; now：正常服务</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

> [1] 目前 Publisher 邀请发布音频流、邀请发布视频流、邀请发布数据流，以及 Subscriber 申请发布音频流、申请发布视频流、申请发布数据流功能尚未实现。
>
> [2] `expireTimestamp` 指 1970 年 1 月 1 日开始到权限到期的秒数。如果想设置 10 分钟的服务有效时间，则输入当前时间戳 + 600（秒）即可。每个权限的有效时间是独立的。

<a name = "removePrivilegeJava"></a>

### 删除用户权限 \(removePrivilege\)

```
public void removePrivilege(AccessToken.Privileges privilege);
```

该方法删除之前设定的用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td>用户权限。详细的权限内容，请见 <a href="#setPrivilegeJava"><span>设置用户权限 (setPrivilege)</span></a></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 生成 Token \(buildToken\)

```java
public String buildToken();
```

该方法创建 Token 。



## C++

### 初始化 Token Builder \(initTokenBuiler\)

```c++
bool initTokenBuilder(const std::string& originToken);
```

该方法初始化 Token Builder。

该方法利用已经存在的 Token，来重新初始化 Token Builder。初始化后，Token Builder 内会包含原来 Token 内的 App ID、App Certificate、 Channel Name、UID 以及 Privilege 信息。

该方法可以结合 [设置用户权限 \(setPrivilege\)](#setPrivilegeCpp) 和 [删除用户权限 \(removePrivilege\)](#removePrivilegeCpp) ，用于为已经存在的 `originToken` 添加、删除权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>originToken</code></td>
<td>用户的初始 Token</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt;0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



Token Builder 的结构体如下所示：

```c++
SimpleTokenBuilder();
SimpleTokenBuilder(const std::string& appId, const std::string& appCertificate,
                   const std::string& channelName, uint32_t uid = 0);
SimpleTokenBuilder(const std::string& appId, const std::string& appCertificate,
                   const std::string& channelName, const std::string& uid = "");
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
<tr><td><code>appID</code></td>
<td>Agora 为应用程序开发者签发的 App ID。详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App ID</span></a>     。</td>
</tr>
<tr><td><code>appCertificate</code></td>
<td>Agora 为应用程序开发者签发的 App Certificate。当启用 App Certificate 后，原有的 App ID 会失效，详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App Certificate</span></a> 。</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户 ID，32 位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护</td>
</tr>
</tbody>
</table>



### 初始化用户权限 \(initPrivileges\)

```c++
bool initPrivileges(Role role);
```

该方法初始化用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>用户角色。根据不同的场景，用户可分为以下角色：</p>
<ul>
<li>Role_Attendee = 0：将用户角色设置为通信场景下参与通话的各方</li>
<li>Role_Publisher = 1：将用户角色设置为直播场景下能发布音视频流的 Publisher</li>
<li>Role_Subscriber = 2：将用户角色设置为直播场景下能订阅音视频流的 Subscriber</li>
<li>Role_Admin = 101：将用户角色设置为通信及直播场景下的管理员</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt;0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>

> 用户角色的设置决定了该角色可以拥有哪些权限。关于角色和权限的对应搭配，请见 [设置用户权限 \(setPrivilege\)](#setPrivilegeCpp) 。

<a name = "setPrivilegeCpp"></a>

### 设置用户权限 \(setPrivilege\)

```c++
void setPrivilege(AccessToken::Privileges privilege, uint32_t expireTimestamp = 0);
```

该方法设置用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>用户权限，共有4种：</p>
<ul>
<li>AttendeePrivileges：通话方权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
</ul>
</li>
<li>PublisherPrivileges：发布者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kPublishAudiocdn：发布音频 CDN</li>
<li>kPublishVideocdn：发布视频 CDN</li>
<li>kInvitePublishAudioStream：邀请发布音频流 <sup>[1]</sup></li>
<li>kInvitePublishVideoStream：邀请发布视频流 <sup>[1]</sup></li>
<li>kInvitePublishDataStream：邀请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>SubscriberPrivileges：订阅者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kRequestPublishAudioStream：申请发布音频流 <sup>[1]</sup></li>
<li>kRequestPublishVideoStream：申请发布视频流 <sup>[1]</sup></li>
<li>kRequestPublishDataStream：申请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>AdminPrivileges：管理员权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kAdministrateChannel：管理频道</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td><p>服务过期时间：</p>
<div><ul>
<li>0（默认）：服务不过期</li>
<li>&lt; now：拒绝登录，强行下线</li>
<li>&gt; now：正常服务</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

> [1] 目前 Publisher 邀请发布音频流、邀请发布视频流、邀请发布数据流，以及 Subscriber 申请发布音频流、申请发布视频流、申请发布数据流功能尚未实现。
>
> [2] `expireTimestamp` 指 1970 年 1 月 1 日开始到权限到期的秒数。如果想设置 10 分钟的服务有效时间，则输入当前时间戳 + 600（秒）即可。每个权限的有效时间是独立的。

<a name = "removePrivilegeCpp"></a>

### 删除用户权限 \(removePrivilege\)

```
void removePrivilege(AccessToken::Privileges privilege);
```

该方法删除之前设定的用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td>用户权限。详细的权限内容，请见 <a href="#setPrivilegeCpp"><span>设置用户权限 (setPrivilege)</span></a></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 生成 Token \(buildToken\)

```c++
std::string buildToken();
```

该方法生成 Token，Token 的类型为 String。



## Python

### 初始化 Token Builder \(initTokenBuiler\)

```python
def initTokenBuilder(self, originToken);
```

该方法初始化 Token Builder。

该方法利用已经存在的 Token，来重新初始化 Token Builder。初始化后，Token Builder 内会包含原来 Token 内的 App ID、App Certificate、 Channel Name、UID 以及 Privilege 信息。

该方法可以结合 [设置用户权限 \(setPrivilege\)](#setPrivilegePython) 和 [删除用户权限 \(removePrivilege\)](#removePrivilegePython) ，用于为已经存在的 originToken 添加、删除权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>originToken</code></td>
<td>用户的初始 Token</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



Token Builder 的结构体如下所示：

```python
def __init__(self, appID, appCertificate, channelName, uid);
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
<tr><td><code>appID</code></td>
<td>Agora 为应用程序开发者签发的 App ID。详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App ID</span></a> 。</td>
</tr>
<tr><td><code>appCertificate</code></td>
<td>Agora 为应用程序开发者签发的 App Certificate。当启用 App Certificate 后，原有的 App ID 会失效，详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App Certificate</span></a> 。</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户ID，32位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护</td>
</tr>
</tbody>
</table>



### 初始化用户权限 \(initPrivileges\)

```python
def initPrivileges(self, role);
```

该方法初始化用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>用户角色。根据不同的场景，用户可分为以下角色：</p>
<ul>
<li>Role_Attendee = 0：将用户角色设置为通信场景下参与通话的各方</li>
<li>Role_Publisher = 1：将用户角色设置为直播场景下能发布音视频流的 Publisher</li>
<li>Role_Subscriber = 2：将用户角色设置为直播场景下能订阅音视频流的 Subscriber</li>
<li>Role_Admin = 101：将用户角色设置为通信及直播场景下的管理员</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>

> 用户角色的设置决定了该角色可以拥有哪些权限。关于角色和权限的对应搭配，请见 [设置用户权限 \(setPrivilege\)](#setPrivilegePython) 。

<a name = "setPrivilegePython" ></a>

### 设置用户权限 \(setPrivilege\)

```python
def setPrivilege(self, privilege, expireTimestamp);
```

该方法设置用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>用户权限，共有4种：</p>
<ul>
<li>AttendeePrivileges：通话方权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
</ul>
</li>
<li>PublisherPrivileges：发布者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kPublishAudiocdn：发布音频 CDN</li>
<li>kPublishVideocdn：发布视频 CDN</li>
<li>kInvitePublishAudioStream：邀请发布音频流 <sup>[1]</sup></li>
<li>kInvitePublishVideoStream：邀请发布视频流 <sup>[1]</sup></li>
<li>kInvitePublishDataStream：邀请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>SubscriberPrivileges：订阅者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kRequestPublishAudioStream：申请发布音频流 <sup>[1]</a></li>
<li>kRequestPublishVideoStream：申请发布视频流 <sup>[1]</a></li>
<li>kRequestPublishDataStream：申请发布数据流 <sup>[1]</a></li>
</ul>
</li>
<li>AdminPrivileges：管理员权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kAdministrateChannel：管理频道</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td><p>服务过期时间：</p>
<div><ul>
<li>0（默认）：服务不过期</li>
<li>&lt; now：拒绝登录，强行下线</li>
<li>&gt; now：正常服务</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

> [1] 目前 Publisher 邀请发布音频流、邀请发布视频流、邀请发布数据流，以及 Subscriber 申请发布音频流、申请发布视频流、申请发布数据流功能尚未实现。
>
> [2] `expireTimestamp` 指 1970 年 1 月 1 日开始到权限到期的秒数。如果想设置 10 分钟的服务有效时间，则输入当前时间戳 + 600（秒）即可。每个权限的有效时间是独立的。

<a name = "removePrivilegePython"></a>

### 删除用户权限 \(removePrivilege\)

```python
def removePrivilege(self, privilege);
```

该方法删除之前设定的用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td>用户权限。详细的权限内容，请见 <a href="#setPrivilegePython"><span>设置用户权限 (setPrivilege)</span></a></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 生成 Token\(buildToken\)

```python
def buildToken(self);
```

该方法创建 Token 。



## Go

### 初始化 Token Builder \(initTokenBuiler\)

```go
func (builder SimpleTokenBuilder) InitTokenBuilder(originToken string);
```

该方法初始化 Token Builder。

该方法利用已经存在的 Token，来重新初始化 Token Builder。初始化后，Token Builder 内会包含原来 Token 内的 App ID、App Certificate、 Channel Name、UID 以及 Privilege 信息。

该方法可以结合 [设置用户权限 \(setPrivilege\)](#setPrivilegeGo) 和 [删除用户权限 \(removePrivilege\)](#removePrivilegeGo) ，用于为已经存在的 `originToken` 添加、删除权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>originToken</code></td>
<td>用户的初始 Token</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt;0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



Token Builder 的结构体如下所示：

```go
func CreateSimpleTokenBuilder(appID, appCertificate, channelName string, uid uint32) SimpleTokenBuilder;
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
<tr><td><code>appID</code></td>
<td>Agora 为应用程序开发者签发的 App ID。详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App ID</span></a> 。</td>
</tr>
<tr><td><code>appCertificate</code></td>
<td>Agora 为应用程序开发者签发的 App Certificate。当启用 App Certificate 后，原有的 App ID 会失效，详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App Certificate</span></a> 。</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户 ID，32 位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护</td>
</tr>
</tbody>
</table>



### 初始化用户权限 \(initPrivileges\)

```go
func (builder SimpleTokenBuilder) InitPrivileges(role Role);
```

该方法初始化用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>用户角色。根据不同的场景，用户可分为以下角色：</p>
<ul>
<li>Role_Attendee = 0：将用户角色设置为通信场景下参与通话的各方</li>
<li>Role_Publisher = 1：将用户角色设置为直播场景下能发布音视频流的 Publisher</li>
<li>Role_Subscriber = 2：将用户角色设置为直播场景下能订阅音视频流的 Subscriber</li>
<li>Role_Admin = 101：将用户角色设置为通信及直播场景下的管理员</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt;0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>

> 用户角色的设置决定了该角色可以拥有哪些权限。关于角色和权限的对应搭配，请见 [设置用户权限 \(setPrivilege\)](#setPrivilegeGo) 。

<a name = "setPrivilegeGo"></a>

### 设置用户权限 \(setPrivilege\)

```go
func (builder SimpleTokenBuilder) SetPrivilege(privilege AccessToken.Privileges, expireTimestamp uint32);
```

该方法设置用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>用户权限，共有4种：</p>
<ul>
<li>AttendeePrivileges：通话方权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
</ul>
</li>
<li>PublisherPrivileges：发布者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kPublishAudiocdn：发布音频 CDN</li>
<li>kPublishVideocdn：发布视频 CDN</li>
<li>kInvitePublishAudioStream：邀请发布音频流 <sup>[1]</sup></li>
<li>kInvitePublishVideoStream：邀请发布视频流 <sup>[1]</sup></li>
<li>kInvitePublishDataStream：邀请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>SubscriberPrivileges：订阅者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kRequestPublishAudioStream：申请发布音频流 <sup>[1]</sup></li>
<li>kRequestPublishVideoStream：申请发布视频流 <sup>[1]</sup></li>
<li>kRequestPublishDataStream：申请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>AdminPrivileges：管理员权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kAdministrateChannel：管理频道</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td><p>服务过期时间：</p>
<div><ul>
<li>0（默认）：服务不过期</li>
<li>&lt; now：拒绝登录，强行下线</li>
<li>&gt; now：正常服务</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>

> [1]  目前 Publisher 邀请发布音频流、邀请发布视频流、邀请发布数据流，以及 Subscriber 申请发布音频流、申请发布视频流、申请发布数据流功能尚未实现。
>
> [2]  `expireTimestamp` 指 1970 年 1 月 1 日开始到权限到期的秒数。如果想设置 10 分钟的服务有效时间，则输入当前时间戳 + 600（秒）即可。每个权限的有效时间是独立的。

<a name = "removePrivilegeGo"></a>

### 删除用户权限 \(removePrivilege\)

```go
func (builder SimpleTokenBuilder) RemovePrivilege(privilege AccessToken.Privileges);
```

该方法删除之前设定的用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code.</td>
<td>用户权限。详细的权限内容，请见 <a href="#setPrivilegeGo"><span>设置用户权限 (setPrivilege)</span></a></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 生成 Token \(buildToken\)

```go
func (builder SimpleTokenBuilder) BuildToken() (string,error);
```

该方法生成 Token。



## PHP

### 初始化 Token Builder \(initTokenBuiler\)

```php
function initTokenBuilder($originToken);
```

该方法初始化 Token Builder。

该方法利用已经存在的 Token，来重新初始化 Token Builder。初始化后，Token Builder 内会包含原来 Token 内的 App ID、App Certificate、 Channel Name、UID 以及 Privilege 信息。

该方法可以结合 [设置用户权限 \(setPrivilege\)](#setPrivilegePHP) 和 [删除用户权限 \(removePrivilege\)](#removePrivilegePHP) ，用于为已经存在的 `originToken` 添加、删除权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
	<tr><td><code>originToken</code></td>
<td>用户的初始 Token</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



Token Builder 的结构体如下所示：

```php
public function __construct($appID, $appCertificate, $channelName, $uid);
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
<tr><td><code>appID</code></td>
<td>Agora 为应用程序开发者签发的 App ID。详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App ID</span></a> 。</td>
</tr>
<tr><td><code>appCertificate</code></td>
<td>Agora 为应用程序开发者签发的 App Certificate。当启用 App Certificate 后，原有的 App ID 会失效，详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App Certificate</span></a> 。</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户ID，32位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护</td>
</tr>
</tbody>
</table>



### 初始化用户权限 \(initPrivilige\)

```php
public function initPrivilige($role);
```

该方法初始化用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>用户角色。根据不同的场景，用户可分为以下角色：</p>
<ul>
<li>RoleAttendee = 0：将用户角色设置为通信场景下参与通话的各方</li>
<li>RolePublisher = 1：将用户角色设置为直播场景下能发布音视频流的 Publisher</li>
<li>RoleSubscriber = 2：将用户角色设置为直播场景下能订阅音视频流的 Subscriber</li>
<li>RoleAdmin = 101：将用户角色设置为通信及直播场景下的管理员</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>

> 用户角色的设置决定了该角色可以拥有哪些权限。关于角色和权限的对应搭配，请见 [设置用户权限 \(setPrivilege\)](#setPrivilegePHP) 。

<a name = "setPrivilege"></a>

### 设置用户权限 \(setPrivilege\)

```php
public function setPrivilege($privilege, $expireTimestamp)
```

该方法设置用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>用户权限，共有4种：</p>
<ul>
<li>AttendeePrivileges：通话方权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
</ul>
</li>
<li>PublisherPrivileges：发布者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kPublishAudiocdn：发布音频 CDN</li>
<li>kPublishVideocdn：发布视频 CDN</li>
<li>kInvitePublishAudioStream：邀请发布音频流 <sup>[1]</sup></li>
<li>kInvitePublishVideoStream：邀请发布视频流 <sup>[1]</sup></li>
<li>kInvitePublishDataStream：邀请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>SubscriberPrivileges：订阅者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kRequestPublishAudioStream：申请发布音频流 <sup>[1]</sup></li>
<li>kRequestPublishVideoStream：申请发布视频流 <sup>[1]</sup></li>
<li>kRequestPublishDataStream：申请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>AdminPrivileges：管理员权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kAdministrateChannel：管理频道</li>
</ul>
</li>
</ul>
</td>
</tr>
	<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td><p>服务过期时间：</p>
<div><ul>
<li>0（默认）：服务不过期</li>
<li>&lt; now：拒绝登录，强行下线</li>
<li>&gt; now：正常服务</li>
</ul>
</div>
</td>
</tr>
</tbody>
</table>



> [1]  目前 Publisher 邀请发布音频流、邀请发布视频流、邀请发布数据流，以及 Subscriber 申请发布音频流、申请发布视频流、申请发布数据流功能尚未实现。
>
> [2]  `expireTimestamp` 指 1970 年 1 月 1 日开始到权限到期的秒数。如果想设置 10 分钟的服务有效时间，则输入当前时间戳 + 600（秒）即可。每个权限的有效时间是独立的。

<a name = "removePrivilegePHP"></a>

### 删除用户权限 \(removePrivilege\)

```php
public function removePriviledge($privilege);
```

该方法删除之前设定的用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td>用户权限。详细的权限内容，请见 <a href="#setPrivilegePHP"><span>设置用户权限 (setPrivilege)</span></a></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 生成 Token \(buildToken\)

```php
public function buildToken();
```

该方法创建 Token 。



## Node.js

### 初始化 Token Builder \(initTokenBuiler\)

```js
initTokenBuilder = function (originToken);
```

该方法初始化 Token Builder。

该方法利用已经存在的 Token，来重新初始化 Token Builder。初始化后，Token Builder 内会包含原来 Token 内的 App ID、App Certificate、 Channel Name、UID 以及 Privilege 信息。

该方法可以结合 [设置用户权限 \(setPrivilege\)](#setPrivilegeNode) 和 [删除用户权限 \(removePrivilege\)](#removePrivilegeNode) ，用于为已经存在的 `originToken` 添加、删除权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>originToken</code></td>
<td>用户的初始 Token</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt;0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



Token Builder 的结构体如下所示：

```javascript
var SimpleTokenBuilder = function (appID, appCertificate, channelName, uid);
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
<tr><td><code>appID</code></td>
<td>Agora 为应用程序开发者签发的 App ID。详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App ID</span></a> 。</td>
</tr>
<tr><td><code>appCertificate</code></td>
<td>Agora 为应用程序开发者签发的 App Certificate。当启用 App Certificate 后，原有的 App ID 会失效，详见 <a href="../../cn/Interactive%20Gaming/token.md"><span>获取 App Certificate</span></a> 。</td>
</tr>
<tr><td><code>channelName</code></td>
<td>标识通话的频道名称，长度在64字节以内的字符串。以下为支持的字符集范围（共89个字符）: a-z,A-Z,0-9,space,! #$%&amp;,()+, -,:;&lt;=.#$%&amp;,()+,-,:;&lt;=.,&gt;?@[],^_,{|},~</td>
</tr>
<tr><td><code>uid</code></td>
<td>用户 ID，32 位无符号整数。建议设置范围：1到 (2^32-1)，并保证唯一性。如果不指定（即设为0），SDK 会自动分配一个，并在 <code>onJoinChannelSuccess</code> 回调方法中返回，App 层必须记住该返回值并维护，SDK 不对该返回值进行维护</td>
</tr>
</tbody>
</table>



### 初始化用户权限 \(initPrivileges\)

```javascript
this.initPrivileges = function (role);
```

该方法初始化用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>role</code></td>
<td><p>用户角色。根据不同的场景，用户可分为以下角色：</p>
<ul>
<li>RoleAttendee = 0：将用户角色设置为通信场景下参与通话的各方</li>
<li>RolePublisher = 1：将用户角色设置为直播场景下能发布音视频流的 Publisher</li>
<li>RoleSubscriber = 2：将用户角色设置为直播场景下能订阅音视频流的 Subscriber</li>
<li>RoleAdmin = 101：将用户角色设置为通信及直播场景下的管理员</li>
</ul>
</td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt;0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>

> 用户角色的设置决定了该角色可以拥有哪些权限。关于角色和权限的对应搭配，请见 [设置用户权限 \(setPrivilege\)](#setPrivilegeNode) 。

<a name = "setPrivilegeNode"></a>

### 设置用户权限 \(setPrivilege\)

```js
this.setPrivilege = function (privilege, expireTimestamp);
```

该方法设置用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td><p>用户权限，共有4种：</p>
<ul>
<li>AttendeePrivileges：通话方权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
</ul>
</li>
<li>PublisherPrivileges：发布者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kPublishAudiocdn：发布音频 CDN</li>
<li>kPublishVideocdn：发布视频 CDN</li>
<li>kInvitePublishAudioStream：邀请发布音频流 <sup>[1]</sup></li>
<li>kInvitePublishVideoStream：邀请发布视频流 <sup>[1]</sup></li>
<li>kInvitePublishDataStream：邀请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>SubscriberPrivileges：订阅者权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kRequestPublishAudioStream：申请发布音频流 <sup>[1]</sup></li>
<li>kRequestPublishVideoStream：申请发布视频流 <sup>[1]</sup></li>
<li>kRequestPublishDataStream：申请发布数据流 <sup>[1]</sup></li>
</ul>
</li>
<li>AdminPrivileges：管理员权限<ul>
<li>kJoinChannel：加入频道</li>
<li>kPublishAudioStream：发布音频流</li>
<li>kPublishVideoStream：发布视频流</li>
<li>kPublishDataStream：发布数据流</li>
<li>kAdministrateChannel：管理频道</li>
</ul>
</li>
</ul>
</td>
</tr>
<tr><td><code>expireTimestamp</code> <sup>[2]</sup></td>
<td><p>服务过期时间：</p>
<div><ul>
<li>0（默认）：服务不过期</li>
<li>&lt; now：拒绝登录，强行下线</li>
<li>&gt; now：正常服务</li>
</ul>
	</div>
</td>
</tr>
</tbody>
</table>



> [1] 目前 Publisher 邀请发布音频流、邀请发布视频流、邀请发布数据流，以及 Subscriber 申请发布音频流、申请发布视频流、申请发布数据流功能尚未实现。
>
> [2] `expireTimestamp` 指 1970 年 1 月 1 日开始到权限到期的秒数。如果想设置 10 分钟的服务有效时间，则输入当前时间戳 + 600（秒）即可。每个权限的有效时间是独立的。

<a name = "removePrivilegeNode"></a>

### 删除用户权限 \(removePrivilege\)

```javascript
this.removePrivilege = function (privilege);
```

该方法删除之前设定的用户权限。

<table>
<colgroup>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>参数</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td><code>privilege</code></td>
<td>用户权限。详细的权限内容，请见 <a href="#setPrivilegeNode"><span>设置用户权限 (setPrivilege)</span></a></td>
</tr>
<tr><td>返回值</td>
<td><ul>
<li>0：方法调用成功</li>
<li>&lt; 0：方法调用不成功</li>
</ul>
</td>
</tr>
</tbody>
</table>



### 生成 Token \(buildToken\)

```javascript
this.buildToken = function ();
```

该方法生成 Token。

