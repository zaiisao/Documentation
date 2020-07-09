
---
title: Error Codes and Warning Codes
description: 
platform: All Platforms
updatedAt: Thu Jul 09 2020 05:32:31 GMT+0800 (CST)
---
# Error Codes and Warning Codes
The Agora Native SDK will return error or warning codes when calling APIs or during runtime:

-   **Error Codes** occur when the SDK encounters an error that cannot be recovered automatically without any application intervention. For example, the SDK returns an error if it fails to turn on the camera, and reminds the user not to use the camera.

-   **Warning Codes** occur when the SDK encounters an error that might be recovered automatically. These are only notifications, and can generally be ignored.


## Error Codes

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Error Code</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>ERR_OK</td>
<td>0</td>
<td>No error.</td>
</tr>
<tr><td>ERR_FAILED</td>
<td>1</td>
<td>General error (the reason is not classified specifically).</td>
</tr>
<tr><td>ERR_INVALID_ARGUMENT</td>
<td>2</td>
<td>Invalid parameter called. For example, the specific channel name includes illegal characters.</td>
</tr>
<tr><td>ERR_NOT_READY</td>
<td>3</td>
<td>The SDK module is not ready. For example, if some API relies on a specific module, and the module is not ready.</td>
</tr>
<tr><td>ERR_NOT_SUPPORTED</td>
<td>4</td>
<td>The SDK does not support this function.</td>
</tr>
<tr><td>ERR_REFUSED</td>
<td>5</td>
<td>The request is rejected. This is for internal SDK internal use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_BUFFER_TOO_SMALL</td>
<td>6</td>
<td>The buffer size is not big enough to store the returned data.</td>
</tr>
<tr><td>ERR_NOT_INITIALIZED</td>
<td>7</td>
<td>The SDK is not initialized before calling this API.</td>
</tr>
<tr><td>ERR_NO_PERMISSION</td>
<td>9</td>
<td>No permission. This is for internal SDK internal use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_TIMEDOUT</td>
<td>10</td>
<td>An API timeout. Some APIs require the SDK to return the execution result, and this error occurs if the request takes too long for the SDK to process.</td>
</tr>
<tr><td>ERR_CANCELED</td>
<td>11</td>
<td>The request is cancelled. This is for internal SDK internal use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_TOO_OFTEN</td>
<td>12</td>
<td>The call frequency is too high. This is for internal SDK internal use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_BIND_SOCKET</td>
<td>13</td>
<td>The SDK failed to bind to the network socket. This is for internal SDK internal use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_NET_DOWN</td>
<td>14</td>
<td>The network is unavailable. This is for internal SDK internal use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_NET_NOBUFS</td>
<td>15</td>
<td>No network buffers available. This is for internal SDK internal use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_JOIN_CHANNEL_REJECTED</td>
<td>17</td>
<td>The request to join the channel is rejected. This error usually occurs when the user is already in the channel, and still calls the API to join the channel, for example, joinChannel.</td>
</tr>
<tr><td>ERR_LEAVE_CHANNEL_REJECTED</td>
<td>18</td>
<td>The request to leave the channel is rejected. This error usually occurs when the user has already left the channel, and still calls the API to leave the channel, for example, leaveChannel.</td>
</tr>
<tr><td>ERR_ALREADY_IN_USE</td>
<td>19</td>
<td>Resources have been occupied, and cannot be reused.</td>
</tr>
<tr><td>ERR_ABORTED</td>
<td>20</td>
<td>The SDK gave up the request due to too many requests. This is for internal SDK internal use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_INIT_NET_ENGINE</td>
<td>21</td>
<td>In Windows, specific firewall settings can cause the SDK to fail to initialize and crash.</td>
</tr>
<tr><td>ERR_INVALID_APP_ID</td>
<td>101</td>
<td>The specified App ID is invalid.</td>
</tr>
<tr><td>ERR_INVALID_TOKEN</td>
<td>102</td>
<td>The specified Token is invalid.</td>
</tr>
<tr><td>ERR_TOKEN_EXPIRED</td>
<td>109</td>
<td><p>The Token expired due to one of the following reasons:</p>
<div><ul>
<li><ol>
<li>Authorized Timestamp expired: The timestamp is represented by the number of seconds elapsed since 1/1/1970. The user can use the Token to access the Agora service within five minutes after the Token is generated. If the user does not access the Agora service after five minutes, this Token will no longer be valid.</li>
</ol>
</li>
<li><ol start="2">
<li>Call Expiration Timestamp expired: The timestamp indicates the exact time when a user can no longer use the Agora service (for example, when a user is forced to leave an ongoing call). When the value is set for the Call Expiration Timestamp, it does not mean that the Dynamic Key will be expired, but that the user will be kicked out of the channel.</li>
</ol>
</li>
</ul>
</div>
</td>
</tr>
<tr><td>ERR_INVALID_TOKEN</td>
<td>110</td>
<td>The Token is invalid due to one of the following reasons: The App Certificate for the project is enabled on the Console, but the user is still using the App ID. Once the App Certificate is enabled, the user must use a Token. The <em>uid</em> field is mandatory, and users must set the same <em>uid</em> when setting the <em>uid</em> parameter when calling joinChannel.</td>
</tr>
<tr><td>ERR_CONNECTION_INTERRUPTED</td>
<td>111</td>
<td>The CONNECTION_INTERRUPTED callback. This applies to the Agora Web SDK only.</td>
</tr>
<tr><td>ERR_CONNECTION_LOST</td>
<td>112</td>
<td>The CONNECTION_LOST callback. This applies to the Agora Web SDK only.</td>
</tr>
<tr><td>ERR_NOT_IN_CHANNEL</td>
<td>113</td>
<td>The user is not in the channel.</td>
</tr>
<tr><td>ERR_SIZE_TOO_LARGE</td>
<td>114</td>
<td>The data size is too big.</td>
</tr>
<tr><td>ERR_BITRATE_LIMIT</td>
<td>115</td>
<td>The bitrate is limited.</td>
</tr>
<tr><td>ERR_TOO_MANY_DATA_STREAMS</td>
<td>116</td>
<td>Too many data streams.</td>
</tr>
<tr><td>ERR_DECRYPTION_FAILED</td>
<td>120</td>
<td>Failed to decrypt.</td>
</tr>
<tr><td>ERR_CLIENT_IS_BANNED_BY_SERVER</td>
<td>123</td>
<td>The client is banned by the server.</td>
</tr>
<tr><td>ERR_WATERMARK_PARAM</td>
<td>124</td>
<td>Error in the watermark file parameter.</td>
</tr>
<tr><td>ERR_WATERMARK_PATH</td>
<td>125</td>
<td>Error in the watermark file path.</td>
</tr>
<tr><td>ERR_WATERMARK_PNG</td>
<td>126</td>
<td>Error in the watermark file format.</td>
</tr>
<tr><td>ERR_WATERMARKR_INFO</td>
<td>127</td>
<td>Error in the watermark file information.</td>
</tr>
<tr><td>ERR_WATERMARK_ARGB</td>
<td>128</td>
<td>Error in the watermark file data format.</td>
</tr>
<tr><td>ERR_WATERMARK_READ</td>
<td>129</td>
<td>Error in reading the watermark file.</td>
</tr>
<tr><td>ERR_LOAD_MEDIA_ENGINE</td>
<td>1001</td>
<td>Failed to load the media engine.</td>
</tr>
<tr><td>ERR_START_CALL</td>
<td>1002</td>
<td>Failed to start the call after enabling the media engine.</td>
</tr>
<tr><td>ERR_START_CAMERA</td>
<td>1003</td>
<td>Failed to start the camera.</td>
</tr>
<tr><td>ERR_START_VIDEO_RENDER</td>
<td>1004</td>
<td>Failed to start the video rendering module.</td>
</tr>
<tr><td>ERR_ADM_GENERAL_ERROR</td>
<td>1005</td>
<td>General error on the Audio Device Module (the reason is not classified specifically.</td>
</tr>
<tr><td>ERR_ADM_JAVA_RESOURCE</td>
<td>1006</td>
<td>Audio Device Module:  Error in using the Java resources.</td>
</tr>
<tr><td>ERR_ADM_SAMPLE_RATE</td>
<td>1007</td>
<td>Audio Device Module: Error in setting the sampling frequency.</td>
</tr>
<tr><td>ERR_ADM_INIT_PLAYOUT</td>
<td>1008</td>
<td>Audio Device Module: Error in initializing the playback device.</td>
</tr>
<tr><td>ERR_ADM_START_PLAYOUT</td>
<td>1009</td>
<td>Audio Device Module: Error in starting the playback device.</td>
</tr>
<tr><td>ERR_ADM_STOP_PLAYOUT</td>
<td>1010</td>
<td>Audio Device Module: Error in stopping the playback device.</td>
</tr>
<tr><td>ERR_ADM_INIT_RECORDING</td>
<td>1011</td>
<td>Audio Device Module: Error in initializing the recording device.</td>
</tr>
<tr><td>ERR_ADM_START_RECORDING</td>
<td>1012</td>
<td>Audio Device Module: Error in starting the recording device.</td>
</tr>
<tr><td>ERR_ADM_STOP_RECORDING</td>
<td>1013</td>
<td>Audio Device Module: Error in stopping the recording device.</td>
</tr>
<tr><td>ERR_ADM_RUNTIME_PLAYOUT_ERROR</td>
<td>1015</td>
<td>Audio Device Module: Runtime playback error.</td>
</tr>
<tr><td>ERR_ADM_RUNTIME_RECORDING_ERROR</td>
<td>1017</td>
<td>Audio Device Module: Runtime recording error.</td>
</tr>
<tr><td>ERR_ADM_RECORD_AUDIO_FAILED</td>
<td>1018</td>
<td>Audio Device Module: Failed to record.</td>
</tr>
<tr><td>ERR_ADM_INIT_LOOPBACK</td>
<td>1022</td>
<td>Audio Device Module: Error in initializing the loopback device.</td>
</tr>
<tr><td>ERR_ADM_START_LOOPBACK</td>
<td>1023</td>
<td>Audio Device Module: Error in starting the loopback device.</td>
</tr>
<tr><td>ERR_ADM_NO_RECORDING_DEVICE</td>
<td>1359</td>
<td>Audio Device Module: No recording device.</td>
</tr>
<tr><td>ERR_ADM_NO_PLAYOUT_DEVICE</td>
<td>1360</td>
<td>Audio Device Module: No playback device.</td>
</tr>
<tr><td>ERR_VDM_CAMERA_NOT_AUTHORIZED</td>
<td>1501</td>
<td>Video Device Module: The camera is not authorized.</td>
</tr>
<tr><td>ERR_VCM_UNKNOWN_ERROR</td>
<td>1600</td>
<td>Video Device Module: Unknown error.</td>
</tr>
<tr><td>ERR_VCM_ENCODER_INIT_ERROR</td>
<td>1601</td>
<td>Video Device Module: Error in initializing the video encoder.</td>
</tr>
<tr><td>ERR_VCM_ENCODER_ENCODE_ERROR</td>
<td>1602</td>
<td>Video Device Module: Error in encoding.</td>
</tr>
<tr><td>ERR_VCM_ENCODER_SET_ERROR</td>
<td>1603</td>
<td>Video Device Module: Error in setting the video encoder.</td>
</tr>
</tbody>
</table>


## Warning Codes

<table>
<colgroup>
<col/>
<col/>
<col/>
</colgroup>
<thead>
<tr><th>Warning Code</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>WARN_INVALID_VIEW</td>
<td>8</td>
<td>The specified view is invalid. It is required to specify a view when using the video call function.</td>
</tr>
<tr><td>WARN_INIT_VIDEO</td>
<td>16</td>
<td>Failed to initialize the video function.</td>
</tr>
<tr><td>WARN_PENDING</td>
<td>20</td>
<td>The request is pending, usually due to some module not being ready, and the SDK postponed processing the request.</td>
</tr>
<tr><td>WARN_NO_AVAILABLE_CHANNEL</td>
<td>103</td>
<td>No channel resources are available. Maybe because the server cannot allocate any channel resource.</td>
</tr>
<tr><td>WARN_LOOKUP_CHANNEL_TIMEOUT</td>
<td>104</td>
<td>A timeout when looking up the channel. When joining a channel, the SDK looks up the specified channel. The warning usually occurs when the network condition is too poor to connect to the server.</td>
</tr>
<tr><td>WARN_LOOKUP_CHANNEL_REJECTED</td>
<td>105</td>
<td>The server rejected the request to look up the channel. The server cannot process this request or the request is illegal.</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_TIMEOUT</td>
<td>106</td>
<td>A timeout when opening the channel. Once the specific channel is found, the SDK opens the channel. The warning usually occurs when the network condition is too poor to connect to the server.</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_REJECTED</td>
<td>107</td>
<td>The server rejected the request to open the channel.  The server cannot process this request or the request is illegal.</td>
</tr>
<tr><td>WARN_SWITCH_LIVE_VIDEO_TIMEOUT</td>
<td>111</td>
<td>A timeout when switching the live video.</td>
</tr>
<tr><td>WARN_SET_CLIENT_ROLE_TIMEOUT</td>
<td>118</td>
<td>A timeout when setting the client role in the live broadcast profile.</td>
</tr>
<tr><td>WARN_SET_CLIENT_ROLE_NOT_AUTHORIZED</td>
<td>119</td>
<td>The client role is not authorized.</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_INVALID_TICKET</td>
<td>121</td>
<td>The ticket to open the channel is invalid.</td>
</tr>
<tr><td>WARN_AUDIO_MIXING_OPEN_ERROR</td>
<td>701</td>
<td>Error in opeinging the audio mixing.</td>
</tr>
<tr><td>WARN_ADM_RUNTIME_PLAYOUT_WARNING</td>
<td>1014</td>
<td>Audio Device Module: A warning in the runtime playback device.</td>
</tr>
<tr><td>WARN_ADM_RUNTIME_RECORDING_WARNING</td>
<td>1016</td>
<td>Audio Device Module: A warning in the runtime recording device.</td>
</tr>
<tr><td>WARN_ADM_RECORD_AUDIO_SILENCE</td>
<td>1019</td>
<td>Audio Device Module: No valid audio data is collected.</td>
</tr>
<tr><td>WARN_ADM_PLAYOUT_MALFUNCTION</td>
<td>1020</td>
<td>Audio Device Module: A playback device failure.</td>
</tr>
<tr><td>WARN_ADM_RECORD_MALFUNCTION</td>
<td>1021</td>
<td>Audio Device Module: A recording device failure.</td>
</tr>
<tr><td>WARN_ADM_RECORD_AUDIO_LOWLEVEL</td>
<td>1031</td>
<td>Audio Device Module: The recorded audio voice is too low.</td>
</tr>
<tr><td>WARN_ADM_PLAYOUT_AUDIO_LOWLEVEL</td>
<td>1032</td>
<td>Audio Device Module: The playback audio voice is too low.</td>
</tr>
<tr><td>WARN_ADM_RECORD_IS_OCCUPIED</td>
<td>1033</td>
<td>Audio Device Module: The recording device is occupied.</td>
</tr>
<tr><td>WARN_ADM_HOWLING</td>
<td>1051</td>
<td>Audio Device Module: Howling is detected.</td>
</tr>
<tr><td>WARN_ADM_GLITCH_STATE</td>
<td>1052</td>
<td>Audio Device Module: The device is in the glitch state.</td>
</tr>
<tr><td>WARN_ADM_IMPROPER_SETTINGS</td>
<td>1053</td>
<td>Audio Device Module: The settings are improper.</td>
</tr>
</tbody>
</table>




