
---
title: 错误代码和警告代码
description: 
platform: Web
updatedAt: Mon Nov 25 2019 10:11:57 GMT+0800 (CST)
---
# 错误代码和警告代码
Agora Web SDK 在调用 API 或运行时，可能会返回错误或警告代码:

-   **错误码** 意味着 SDK 遭遇不可恢复的错误，需要应用程序干预，例如打开摄像头失败会返回错误，应用程序需要提示用户不能使用摄像头。

-   **警告代码** 意味着 SDK 遇到问题，但有可能恢复，警告代码仅起告知作用，一般情况下应用程序可以忽略警告代码。

## 错误码

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>错误码</strong></td>
<td><strong>值</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>STREAM_ALREADY_PUBLISHED</td>
<td>/</td>
<td>重复 Publish</td>
</tr>
<tr><td>INVALID_LOCAL_STREAM</td>
<td>/</td>
<td>Stream 不合法</td>
</tr>
<tr><td>INVALID_OPERATION</td>
<td>/</td>
<td>未加入频道，或重复加入频道</td>
</tr>
<tr><td>PUBLISH_STREAM_FAILED</td>
<td>/</td>
<td>Publish 失败</td>
</tr>
<tr><td>PEERCONNECTION_FAILED</td>
<td>/</td>
<td>P2P 连接断开</td>
</tr>
<tr><td>STREAM_NOT_YET_PUBLISHED</td>
<td>/</td>
<td>尚未 Publish</td>
</tr>
<tr><td>UNPUBLISH_STREAM_FAILED</td>
<td>/</td>
<td>Unpublish 失败</td>
</tr>
<tr><td>INVALID_REMOTE_STREAM</td>
<td>/</td>
<td>远端 Stream 不合法</td>
</tr>
<tr><td>SUBSCRIBE_STREAM_FAILED</td>
<td>/</td>
<td>Subscribe 失败</td>
</tr>
<tr><td>NO_SUCH_REMOTE_STREAM</td>
<td>/</td>
<td>频道里没有这个 Remote 流</td>
</tr>
<tr><td>UNSUBSCRIBE_STREAM_FAILED</td>
<td>/</td>
<td>Unsubscribe 失败</td>
</tr>
<tr><td>INVALID_PARAMETER</td>
<td>/</td>
<td>参数类型不正确</td>
</tr>
<tr><td>JOIN_TOO_FREQUENT</td>
<td>/</td>
<td>加入频道过于频繁</td>
</tr>
<tr><td>IOS_NOT_SUPPORT</td>
<td>/</td>
<td>iOS Safari 不支持小流</td>
</tr>
<tr><td>STILL_ON_PUBLISHING</td>
<td>/</td>
<td>无法在发流的过程中，打开或者关闭小流</td>
</tr>
<tr><td>ENABLE_DUALSTREAM_FAILED</td>
<td>/</td>
<td>打开小流失败</td>
</tr>
<tr><td>SHARING_SCREEN_NOT_SUPPORT</td>
<td>/</td>
<td>屏幕共享不支持小流</td>
</tr>
<tr><td>LOW_STREAM_ALREADY_PUBLISHED</td>
<td>/</td>
<td>小流已经 Publish</td>
</tr>
<tr><td>HIGH_STREAM_NOT_VIDEO_TRACE</td>
<td>/</td>
<td>无法获取大流的轨道</td>
</tr>
<tr><td>NOT_FIND_DEVICE_BY_LABEL</td>
<td>/</td>
<td>无法找到大流的设备</td>
</tr>
<tr><td>DISABLE_DUALSTREAM_FAILED</td>
<td>/</td>
<td>关闭小流失败</td>
</tr>
<tr><td>LOW_STREAM_NOT_YET_PUBLISHED</td>
<td>/</td>
<td>小流没有 Publish</td>
</tr>
<tr><td>ERR_JOIN_CHANNEL_TIMEOUT</td>
<td>2002</td>
<td>加入频道超时</td>
</tr>
<tr><td>ERR_FAILED</td>
<td>1</td>
<td>一般性的错误 (没有明确归类的错误原因)</td>
</tr>
<tr><td>ERR_INVALID_VENDOR_KEY</td>
<td>101</td>
<td>App ID 无效</td>
</tr>
<tr><td>ERR_INVALID_CHANNEL_NAME</td>
<td>102</td>
<td>指定的频道名无效</td>
</tr>
<tr><td>ERR_DYNAMIC_KEY_TIMEOUT</td>
<td>109</td>
<td>Channel Key 或者 Token 失效。生成密钥后需要在一天内使用，超过一天后使用会出现该错误。</td>
</tr>
<tr><td>ERR_NO_AUTHORIZED</td>
<td>110</td>
<td>Channel Key 或者 Token 未经授权</td>
</tr>
<tr><td>ERR_NO_ACTIVE_STATUS</td>
<td>116</td>
<td>厂商注册的 App ID 未激活</td>
</tr>
<tr><td>ERR_INVALID_UID</td>
<td>117</td>
<td>非法的 UID</td>
</tr>
<tr><td>ERR_DYNAMIC_KEY_EXPIRED</td>
<td>118</td>
<td>Channel Key 或者 Token 服务过期</td>
</tr>
<tr><td>ERR_STATIC_USE_DYNAMIC_KEY</td>
<td>119</td>
<td>静态厂商使用了动态 Key</td>
</tr>
<tr><td>ERR_DYNAMIC_USE_STATIC_KEY</td>
<td>120</td>
<td>动态厂商使用了静态 Key</td>
</tr>
<tr><td>K_TIMESTAMP_EXPIRED</td>
<td>2</td>
<td>Channel Key 或者 Token 服务过期</td>
</tr>
<tr><td>K_CHANNEL_PERMISSION_INVALID</td>
<td>3</td>
<td>无权利加入频道，具体解决办法请咨询频道管理员</td>
</tr>
<tr><td>K_CERTIFICATE_INVALID</td>
<td>4</td>
<td>App 证书无效</td>
</tr>
<tr><td>K_CHANNEL_NAME_EMPTY</td>
<td>5</td>
<td>频道名为空</td>
</tr>
<tr><td>K_CHANNEL_NOT_FOUND</td>
<td>6</td>
<td>未找到指定的频道</td>
</tr>
<tr><td>K_TICKET_INVALID</td>
<td>7</td>
<td>凭证无效</td>
</tr>
<tr><td>K_CHANNEL_CONFLICTED</td>
<td>8</td>
<td>频道有冲突</td>
</tr>
<tr><td>K_SERVICE_NOT_READY</td>
<td>9</td>
<td>无法提供服务</td>
</tr>
<tr><td>K_SERVICE_TOO_HEAVY</td>
<td>10</td>
<td>服务负载过大</td>
</tr>
<tr><td>K_UID_BANNED</td>
<td>14</td>
<td>UID 被禁用</td>
</tr>
<tr><td>K_IP_BANNED</td>
<td>15</td>
<td>IP 被禁用</td>
</tr>
<tr><td>K_CHANNEL_BANNED</td>
<td>16</td>
<td>频道被禁用</td>
</tr>
</tbody>
</table>



## 警告代码

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>警告代码</strong></td>
<td><strong>值</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>WARN_NO_AVAILABLE_CHANNEL</td>
<td>103</td>
<td>没有可用的频道资源。可能是因为服务端没法分配频道资源</td>
</tr>
<tr><td>WARN_LOOKUP_CHANNEL_TIMEOUT</td>
<td>104</td>
<td>查找频道超时。在加入频道时 SDK 先要查找指定的频道，出现该警告一般是因为网络太差，连接不到服务器</td>
</tr>
<tr><td>WARN_LOOKUP_CHANNEL_REJECTED</td>
<td>105</td>
<td>查找频道请求被服务器拒绝。服务器可能没有办法处理这个请求或请求是非法的</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_TIMEOUT</td>
<td>106</td>
<td>打开频道超时。查找到指定频道后，SDK 接着打开该频道，超时一般是因为网络太差，连接不到服务器</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_REJECTED</td>
<td>107</td>
<td>打开频道请求被服务器拒绝。服务器可能没有办法处理该请求或该请求是非法的</td>
</tr>
<tr><td>WARN_REQUEST_DEFERRED</td>
<td>108</td>
<td>用户申请延迟</td>
</tr>
</tbody>
</table>




