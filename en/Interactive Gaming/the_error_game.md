
---
title: Error Codes and Warning Codes
description: 
platform: All Platforms
updatedAt: Fri Nov 02 2018 04:24:00 GMT+0800 (CST)
---
# Error Codes and Warning Codes
The Agora Interactive Gaming SDK will return error or warning codes when calling APIs or during runtime:

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
<td>The SDK module is not ready. For example, some API relies on a specific module, but the module is not ready yet.</td>
</tr>
<tr><td>ERR_NOT_SUPPORTED</td>
<td>4</td>
<td>The SDK does not support this function.</td>
</tr>
<tr><td>ERR_REFUSED</td>
<td>5</td>
<td>The request is rejected. This is for internal SDK use only, and it will not return to the application through any API or callback event.</td>
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
<td>No permission. This is for internal SDK use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_TIMEDOUT</td>
<td>10</td>
<td>API timeout. The API requires the SDK to return the execution result, and this error occurs if the request takes too long for the SDK to process.</td>
</tr>
<tr><td>ERR_CANCELED</td>
<td>11</td>
<td>The request is cancelled. This is for internal SDK use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_TOO_OFTEN</td>
<td>12</td>
<td>The call frequency is too high. This is for internal SDK use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_BIND_SOCKET</td>
<td>13</td>
<td>The SDK failed to bind to the network socket. This is for internal SDK use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_NET_DOWN</td>
<td>14</td>
<td>The network is unavailable. This is for internal SDK use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_NET_NOBUFS</td>
<td>15</td>
<td>No network buffers are available. This is for internal SDK use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_JOIN_CHANNEL_REJECTED</td>
<td>17</td>
<td>The request to join the channel is rejected. This error usually occurs when the user is already in the channel, and still calls the API to join the channel.</td>
</tr>
<tr><td>ERR_LEAVE_CHANNEL_REJECTED</td>
<td>18</td>
<td>The request to leave the channel is rejected. This error usually occurs when the user has already left the channel, and still calls the API to leave the channel.</td>
</tr>
<tr><td>ERR_ALREADY_IN_USE</td>
<td>19</td>
<td>The resources have been occupied and cannot be reused.</td>
</tr>
<tr><td>ERR_ABORTED</td>
<td>20</td>
<td>The SDK gave up the request due to too many requests. This is for internal SDK use only, and it will not return to the application through any API or callback event.</td>
</tr>
<tr><td>ERR_INIT_NET_ENGINE</td>
<td>21</td>
<td>In Windows, specific firewall settings can cause the SDK to fail to initialize and crash.</td>
</tr>
<tr><td>ERR_INVALID_VENDOR_KEY</td>
<td>101</td>
<td>The specified App ID is invalid.</td>
</tr>
<tr><td>ERR_INVALID_CHANNEL_NAME</td>
<td>102</td>
<td>The specified Channel Name is invalid.</td>
</tr>
<tr><td>ERR_CONNECTION_INTERRUPTED</td>
<td>111</td>
<td><p>The CONNECTION_INTERRUPTED callback.</p>
<p>This is only applicable to the Agora Native SDK for the web.</p>
</td>
</tr>
<tr/>
<tr><td>ERR_CONNECTION_LOST = 112</td>
<td>112</td>
<td><p>The CONNECTION_LOST callback.</p>
<p>This is only applicable to the Agora Native SDK for the web.</p>
</td>
</tr>
<tr/>
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
<tr><td>ERR_LOAD_MEDIA_ENGINE</td>
<td>1001</td>
<td>Failed to load the media engine.</td>
</tr>
<tr><td>ERR_START_CALL</td>
<td>1002</td>
<td>Failed to start the call after enabling the media engine.</td>
</tr>
<tr><td>ERR_ADM_GENERAL_ERROR</td>
<td>1005</td>
<td>General error in the Audio Device Module (the reason is not classified specifically).</td>
</tr>
<tr><td>ERR_ADM_JAVA_RESOURCE</td>
<td>1006</td>
<td>Audio Device Module: Error in using Java resources.</td>
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
<tr><td>ERR_ADM_NO_PERMISSION</td>
<td>1027</td>
<td>Audio Device Module: The permission to use the Audio Device Module is disabled.</td>
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
<tr><td>ERR_ADM_NO_PERMISSION</td>
<td>1027</td>
<td>Audio Device Module: The permission to use the Audio Device Module is disabled.</td>
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
<tr><td>WARN_PENDING</td>
<td>20</td>
<td>The request is pending, usually due to some module not being ready.</td>
</tr>
<tr><td>WARN_NO_AVAILABLE_CHANNEL</td>
<td>103</td>
<td>No channel resources are available. Maybe because the server could not allocate any channel resource.</td>
</tr>
<tr><td>WARN_LOOKUP_CHANNEL_TIMEOUT</td>
<td>104</td>
<td>A timeout when looking up the channel. When joining a channel, the SDK looks up the specified channel. The warning usually occurs when the network condition is poor to connect to the server.</td>
</tr>
<tr><td>WARN_LOOKUP_CHANNEL_REJECTED</td>
<td>105</td>
<td>The server rejected the request to look up the channel. The server cannot process this request or the request is illegal.</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_TIMEOUT</td>
<td>106</td>
<td>A timeout when opening the channel. Once the specific channel is found, the SDK opens the channel. The warning usually occurs when the network condition is poor to connect to the server.</td>
</tr>
<tr><td>WARN_OPEN_CHANNEL_REJECTED</td>
<td>107</td>
<td>The server rejected the request to open the channel. The server cannot process this request or the request is illegal.</td>
</tr>
<tr><td>WARN_SET_CLIENT_ROLE_TIMEOUT</td>
<td>118</td>
<td>A timeout when setting the client. The server cannot process this request or the request is illegal.</td>
</tr>
<tr><td>WARN_SET_CLIENT_ROLE_NOT_AUTHORIZED</td>
<td>119</td>
<td>The user is not authorized to perform the action.</td>
</tr>
<tr><td>WARN_AUDIO_MIXING_OPEN_ERROR</td>
<td>701</td>
<td>An incorrect or incomplete file is added when calling <em>startAudioMixing()</em>.</td>
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
<tr><td>WARN_ADM_RECORD_MALFUNCTION</td>
<td>1031</td>
<td>Audio Device Module: Low recording volume.</td>
</tr>
<tr><td>WARN_ADM_HOWLING</td>
<td>1051</td>
<td>Audio Device Module: Howling is detected.</td>
</tr>
</tbody>
</table>




