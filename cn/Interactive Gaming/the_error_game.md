
---
title: 错误代码和警告代码
description: 
platform: All Platforms
updatedAt: Mon Nov 25 2019 10:14:01 GMT+0800 (CST)
---
# 错误代码和警告代码
Agora SDK 在调用 API 或运行时，可能会返回错误或警告码:

-   **错误码** 意味着 SDK 遭遇不可恢复的错误，需要应用程序干预，例如打开摄像头失败会返回错误，应用程序需要提示用户不能使用摄像头。

-   **警告码** 意味着 SDK 遇到问题，但有可能恢复，警告码仅起告知作用，一般情况下应用程序可以忽略警告码。


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
<tr><td>ERR_OK</td>
<td>0</td>
<td>没有错误。</td>
</tr>
<tr><td>ERR_FAILED</td>
<td>1</td>
<td>一般性的错误(没有明确归类的错误原因)。</td>
</tr>
<tr><td>ERR_INVALID_ARGUMENT</td>
<td>2</td>
<td>API 调用了无效的参数。例如指定的频道名含有非法字符。</td>
</tr>
<tr><td>ERR_NOT_READY</td>
<td>3</td>
<td>SDK 的模块没有准备好，例如某个 API 调用依赖于某个模块，但该模块尚未准备提供服务。</td>
</tr>
<tr><td>ERR_NOT_SUPPORTED</td>
<td>4</td>
<td>SDK 不支持该功能。</td>
</tr>
<tr><td>ERR_REFUSED</td>
<td>5</td>
<td>调用被拒绝。仅供 SDK 内部使用，不通过 API 或者回调事件返回给应用程序。</td>
</tr>
<tr><td>ERR_BUFFER_TOO_SMALL</td>
<td>6</td>
<td>传入的缓冲区大小不足以存放返回的数据。</td>
</tr>
<tr><td>ERR_NOT_INITIALIZED</td>
<td>7</td>
<td>SDK 尚未初始化，就调用其 API。</td>
</tr>
<tr><td>ERR_NO_PERMISSION</td>
<td>9</td>
<td>没有操作权限。仅供 SDK 内部使用，不通过 API 或者回调事件返回给应用程序。</td>
</tr>
<tr><td>ERR_TIMEDOUT</td>
<td>10</td>
<td>API 调用超时。有些 API 调用需要 SDK 返回结果，如果 SDK 处理时间过长，会出现此错误。</td>
</tr>
<tr><td>ERR_CANCELED</td>
<td>11</td>
<td>请求被取消。仅供 SDK 内部使用，不通过 API 或者回调事件返回给应用程序。</td>
</tr>
<tr><td>ERR_TOO_OFTEN</td>
<td>12</td>
<td>调用频率太高。仅供 SDK 内部使用，不通过 API 或者回调事件返回给应用程序。</td>
</tr>
<tr><td>ERR_BIND_SOCKET</td>
<td>13</td>
<td>SDK 内部绑定到网络 socket 失败。仅供 SDK 内部使用，不通过 API 或者回调事件返回给应用程序。</td>
</tr>
<tr><td>ERR_NET_DOWN</td>
<td>14</td>
<td>网络不可用。仅供 SDK 内部使用，不通过 API 或者回调事件返回给应用程序。</td>
</tr>
<tr><td>ERR_NET_NOBUFS</td>
<td>15</td>
<td>没有网络缓冲区可用。仅供 SDK 内部使用，不通过 API 或者回调事件返回给应用程序。</td>
</tr>
<tr><td>ERR_JOIN_CHANNEL_REJECTED</td>
<td>17</td>
<td>加入频道被拒绝。一般是因为用户已进入频道，再次调用加入频道的 API，例如 joinChannel，会返回此错误。</td>
</tr>
<tr><td>ERR_LEAVE_CHANNEL_REJECTED</td>
<td>18</td>
<td>离开频道失败。一般是因为用户已离开某频道，再次调用退出频道的API，例如 leaveChannel，会返回此错误。</td>
</tr>
<tr><td>ERR_ALREADY_IN_USE</td>
<td>19</td>
<td>资源已被占用，不能重复使用。</td>
</tr>
<tr><td>ERR_ABORTED</td>
<td>20</td>
<td>SDK 放弃请求，可能由于请求次数太多。仅供 SDK 内部使用，不通过 API 或者回调事件返回给应用程序。</td>
</tr>
<tr><td>ERR_INIT_NET_ENGINE</td>
<td>21</td>
<td>Windows 下特定的防火墙设置导致 SDK 初始化失败然后崩溃。</td>
</tr>
<tr><td>ERR_INVALID_VENDOR_KEY</td>
<td>101</td>
<td>指定的 App ID 无效。</td>
</tr>
<tr><td>ERR_INVALID_CHANNEL_NAME</td>
<td>102</td>
<td>指定的频道名无效。</td>
</tr>
<tr><td>ERR_NOT_IN_CHANNEL</td>
<td>113</td>
<td>用户不在频道内。</td>
</tr>
<tr><td>ERR_SIZE_TOO_LARGE</td>
<td>114</td>
<td>数据太大</td>
</tr>
<tr><td>ERR_BITRATE_LIMIT</td>
<td>115</td>
<td>码率受限</td>
</tr>
<tr><td>ERR_LOAD_MEDIA_ENGINE</td>
<td>1001</td>
<td>加载媒体引擎失败。</td>
</tr>
<tr><td>ERR_START_CALL</td>
<td>1002</td>
<td>启动媒体引擎开始通话失败。</td>
</tr>
<tr><td>ERR_ADM_GENERAL_ERROR</td>
<td>1005</td>
<td>音频设备模块出现一般性错误（没有明显归类的错误）。</td>
</tr>
<tr><td>ERR_ADM_JAVA_RESOURCE</td>
<td>1006</td>
<td>语音模块: 使用 java 资源出现错误。</td>
</tr>
<tr><td>ERR_ADM_SAMPLE_RATE</td>
<td>1007</td>
<td>语音模块: 设置的采样频率出现错误。</td>
</tr>
<tr><td>ERR_ADM_INIT_PLAYOUT</td>
<td>1008</td>
<td>语音模块: 初始化播放设备出现错误。</td>
</tr>
<tr><td>ERR_ADM_START_PLAYOUT</td>
<td>1009</td>
<td>语音模块: 启动播放设备出现错误。</td>
</tr>
<tr><td>ERR_ADM_STOP_PLAYOUT</td>
<td>1010</td>
<td>语音模块: 停止播放设备出现错误。</td>
</tr>
<tr><td>ERR_ADM_INIT_RECORDING</td>
<td>1011</td>
<td>语音模块: 初始化录音设备时出现错误。</td>
</tr>
<tr><td>ERR_ADM_START_RECORDING</td>
<td>1012</td>
<td>语音模块: 启动录音设备出现错误。</td>
</tr>
<tr><td>ERR_ADM_STOP_RECORDING</td>
<td>1013</td>
<td>语音模块: 停止录音设备出现错误。</td>
</tr>
<tr><td>ERR_ADM_RUNTIME_PLAYOUT_ERROR</td>
<td>1015</td>
<td>语音模块: 运行时播放出现错误。</td>
</tr>
<tr><td>ERR_ADM_RUNTIME_RECORDING_ERROR</td>
<td>1017</td>
<td>语音模块: 运行时录音错误。</td>
</tr>
<tr><td>ERR_ADM_RECORD_AUDIO_FAILED</td>
<td>1018</td>
<td>语音模块: 录制失败</td>
</tr>
<tr><td>ERR_ADM_INIT_LOOPBACK</td>
<td>1022</td>
<td>语音模块: 初始化 loopback 设备错误。</td>
</tr>
<tr><td>ERR_ADM_START_LOOPBACK</td>
<td>1023</td>
<td>语音模块: 启动 loopback 设备错误。</td>
</tr>
<tr><td>ERR_ADM_NO_PERMISSION</td>
<td>1027</td>
<td>语音模块: 没有使用 ADM 的权限</td>
</tr>
<tr><td>ERR_ADM_RUNTIME_PLAYOUT_ERROR</td>
<td>1015</td>
<td>语音模块: 运行时播放出现错误。</td>
</tr>
<tr><td>ERR_ADM_RUNTIME_RECORDING_ERROR</td>
<td>1017</td>
<td>语音模块: 运行时录音错误。</td>
</tr>
<tr><td>ERR_ADM_RECORD_AUDIO_FAILED</td>
<td>1018</td>
<td>语音模块: 录制失败</td>
</tr>
<tr><td>ERR_ADM_INIT_LOOPBACK</td>
<td>1022</td>
<td>语音模块: 初始化 loopback 设备错误。</td>
</tr>
<tr><td>ERR_ADM_START_LOOPBACK</td>
<td>1023</td>
<td>语音模块: 启动 loopback 设备错误。</td>
</tr>
<tr><td>ERR_ADM_NO_PERMISSION</td>
<td>1027</td>
<td>语音模块: 没有使用 ADM 的权限</td>
</tr>
</tbody>
</table>



## 警告码

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<tbody>
<tr><td><strong>警告码</strong></td>
<td><strong>值</strong></td>
<td><strong>描述</strong></td>
</tr>
<tr><td>WARN_PENDING</td>
<td>20</td>
<td>请求处于待定状态。一般是由于某个模块还没准备好，请求被延迟处理。</td>
</tr>
<tr><td>WARN_NO_AVAILABLE_CHANNEL</td>
<td>103</td>
<td>没有可用的频道资源。可能是因为服务端没法分配频道资源。</td>
</tr>
<tr><td>WARN_LOOKUP_CHANNEL_TIMEOUT</td>
<td>104</td>
<td>查找频道超时。在加入频道时SDK先要查找指定的频道，出现该警告一般是因为网络太差，连接不到服务器。</td>
</tr>
<tr><td>WARN_LOOKUP_CHANNEL_REJECTED</td>
<td>105</td>
<td>查找频道请求被服务器拒绝。服务器可能没有办法处理这个请求或请求是非法的。</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_TIMEOUT</td>
<td>106</td>
<td>打开频道超时。查找到指定频道后，SDK 接着打开该频道，超时一般是因为网络太差，连接不到服务器。</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_REJECTED</td>
<td>107</td>
<td>打开频道请求被服务器拒绝。服务器可能没有办法处理该请求或该请求是非法的。</td>
</tr>
<tr><td>WARN_SET_CLIENT_ROLE_TIMEOUT</td>
<td>118</td>
<td>设置用户角色超时。服务器可能没有办法处理该请求或该请求是非法的。</td>
</tr>
<tr><td>WARN_SET_CLIENT_ROLE_NOT_AUTHORIZED</td>
<td>119</td>
<td>用户没有权限进行该操作</td>
</tr>
<tr><td>WARN_AUDIO_MIXING_OPEN_ERROR</td>
<td>701</td>
<td>调用 startAudioMixing() 时传入了不正确或不完整的文件</td>
</tr>
<tr><td>WARN_ADM_RUNTIME_PLAYOUT_WARNING</td>
<td>1014</td>
<td>音频设备模块: 运行时播放设备出现警告。</td>
</tr>
<tr><td>WARN_ADM_RUNTIME_RECORDING_WARNING</td>
<td>1016</td>
<td>音频设备模块: 运行时录音设备出现警告。</td>
</tr>
<tr><td>WARN_ADM_RECORD_AUDIO_SILENCE</td>
<td>1019</td>
<td>音频设备模块: 没有采集到有效的声音数据。</td>
</tr>
<tr><td>WARN_ADM_PLAYOUT_MALFUNCTION</td>
<td>1020</td>
<td>音频设备模块: 播放设备出现故障。</td>
</tr>
<tr><td>WARN_ADM_RECORD_MALFUNCTION</td>
<td>1021</td>
<td>音频设备模块: 录制设备出现故障。</td>
</tr>
<tr><td>WARN_ADM_RECORD_MALFUNCTION</td>
<td>1031</td>
<td>音频设备模块: 录制声音过小</td>
</tr>
<tr><td>WARN_ADM_HOWLING</td>
<td>1051</td>
<td>音频设备模块: 检测到啸叫。</td>
</tr>
</tbody>
</table>




