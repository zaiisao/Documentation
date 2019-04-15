
---
title: Error Codes and Warning Codes
description: 
platform: Web
updatedAt: Thu Nov 08 2018 10:47:33 GMT+0800 (CST)
---
# Error Codes and Warning Codes
The Agora Web SDK will return error or warning codes when calling APIs or during runtime:

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
<tr><th>Error Code</td>
<th>Value</td>
<th>Description</td>
</tr>
</thead>
<tbody>
<tr><td>STREAM_ALREADY_PUBLISHED</td>
<td>/</td>
<td>The stream has already been published.</td>
</tr>
<tr><td>INVALID_LOCAL_STREAM</td>
<td>/</td>
<td>The stream is illegal.</td>
</tr>
<tr><td>INVALID_OPERATION</td>
<td>/</td>
<td>The user has failed to join the channel, or joined the same channel.</td>
</tr>
<tr><td>PUBLISH_STREAM_FAILED</td>
<td>/</td>
<td>The user has failed to publish the stream.</td>
</tr>
<tr><td>PEERCONNECTION_FAILED</td>
<td>/</td>
<td>P2P connection failed</td>
</tr>
<tr><td>STREAM_NOT_YET_PUBLISHED</td>
<td>/</td>
<td>The stream has not yet been published.</td>
</tr>
<tr><td>UNPUBLISH_STREAM_FAILED</td>
<td>/</td>
<td>The stream has failed to be unpublished.</td>
</tr>
<tr><td>INVALID_REMOTE_STREAM</td>
<td>/</td>
<td>The remote stream is illegal.</td>
</tr>
<tr><td>SUBSCRIBE_STREAM_FAILED</td>
<td>/</td>
<td>The user has failed to subscribe to a stream.</td>
</tr>
<tr><td>NO_SUCH_REMOTE_STREAM</td>
<td>/</td>
<td>There is no such remote stream in the channel.</td>
</tr>
<tr><td>UNSUBSCRIBE_STREAM_FAILED</td>
<td>/</td>
<td>The user has failed to unsubscribe from a stream.</td>
</tr>
<tr><td>INVALID_PARAMETER</td>
<td>/</td>
<td>Incorrect parameter type</td>
</tr>
<tr><td>JOIN_TOO_FREQUENT</td>
<td>/</td>
<td>The request for joining a channel is too frequent.</td>
</tr>
<tr><td>IOS_NOT_SUPPORT</td>
<td>/</td>
<td>The iOS Safari browser does not support the low stream.</td>
</tr>
<tr><td>STILL_ON_PUBLISHING</td>
<td>/</td>
<td>Cannot enable or disable the low stream when publishing the stream</td>
</tr>
<tr><td>ENABLE_DUALSTREAM_FAILED</td>
<td>/</td>
<td>Failed to enable the low stream</td>
</tr>
<tr><td>SHARING_SCREEN_NOT_SUPPORT</td>
<td>/</td>
<td>The screen-share function does not support the low stream</td>
</tr>
<tr><td>LOW_STREAM_ALREADY_PUBLISHED</td>
<td>/</td>
<td>The low stream has already been published.</td>
</tr>
<tr><td>HIGH_STREAM_NOT_VIDEO_TRACE</td>
<td>/</td>
<td>Unable to obtain the channel of the high stream</td>
</tr>
<tr><td>NOT_FIND_DEVICE_BY_LABEL</td>
<td>/</td>
<td>Unable to find the device of the high stream</td>
</tr>
<tr><td>DISABLE_DUALSTREAM_FAILED</td>
<td>/</td>
<td>Failed to disable the low stream</td>
</tr>
<tr><td>LOW_STREAM_NOT_YET_PUBLISHED</td>
<td>/</td>
<td>The low stream has not been published.</td>
</tr>
<tr><td>ERR_JOIN_CHANNEL_TIMEOUT</td>
<td>2002</td>
<td>Timeout in joining the channel</td>
</tr>
<tr><td>ERR_FAILED</td>
<td>1</td>
<td>General error (the reason is not classified specifically)</td>
</tr>
<tr><td>ERR_INVALID_VENDOR_KEY</td>
<td>101</td>
<td>The specified App ID is invalid.</td>
</tr>
<tr><td>ERR_INVALID_CHANNEL_NAME</td>
<td>102</td>
<td>The specified Channel Name is invalid.</td>
</tr>
<tr><td>ERR_DYNAMIC_KEY_TIMEOUT</td>
<td>109</td>
<td>The current Channel Key or Token is invalid. This error occurs if you use the generated Channel Key or Token after one day.</td>
</tr>
<tr><td>ERR_NO_AUTHORIZED</td>
<td>110</td>
<td>The current Channel Key is not authorized.</td>
</tr>
<tr><td>ERR_NO_ACTIVE_STATUS</td>
<td>116</td>
<td>The current App ID is not activated.</td>
</tr>
<tr><td>ERR_INVALID_UID</td>
<td>117</td>
<td>The UID is invalid.</td>
</tr>
<tr><td>ERR_DYNAMIC_KEY_EXPIRED</td>
<td>118</td>
<td>The current Channel Key or Token has expired, and is no longer valid.</td>
</tr>
<tr><td>ERR_STATIC_USE_DYNAMIC_KEY</td>
<td>119</td>
<td>The static user has used the dynamic key.</td>
</tr>
<tr><td>ERR_DYNAMIC_USE_STATIC_KEY</td>
<td>120</td>
<td>The dynamic user has used the static key.</td>
</tr>
<tr><td>K_TIMESTAMP_EXPIRED</td>
<td>2</td>
<td>The Channel Key or Token has expired, and is no longer valid.</td>
</tr>
<tr><td>K_CHANNEL_PERMISSION_INVALID</td>
<td>3</td>
<td>The user has no permission to join the channel. Consult the channel manager to solve this issue.</td>
</tr>
<tr><td>K_CERTIFICATE_INVALID</td>
<td>4</td>
<td>The App Certificate is invalid.</td>
</tr>
<tr><td>K_CHANNEL_NAME_EMPTY</td>
<td>5</td>
<td>The Channel Name is empty.</td>
</tr>
<tr><td>K_CHANNEL_NOT_FOUND</td>
<td>6</td>
<td>The specified channnel is not found.</td>
</tr>
<tr><td>K_TICKET_INVALID</td>
<td>7</td>
<td>The ticket is invalid.</td>
</tr>
<tr><td>K_CHANNEL_CONFLICTED</td>
<td>8</td>
<td>The channel is conflicted.</td>
</tr>
<tr><td>K_SERVICE_NOT_READY</td>
<td>9</td>
<td>The service is not ready.</td>
</tr>
<tr><td>K_SERVICE_TOO_HEAVY</td>
<td>10</td>
<td>The service is too busy.</td>
</tr>
<tr><td>K_UID_BANNED</td>
<td>14</td>
<td>The UID is banned.</td>
</tr>
<tr><td>K_IP_BANNED</td>
<td>15</td>
<td>The IP is banned.</td>
</tr>
<tr><td>K_CHANNEL_BANNED</td>
<td>16</td>
<td>The channel is banned.</td>
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
<tr><th>Warn Code</th>
<th>Value</th>
<th>Description</th>
</tr>
</thead>
<tbody>
<tr><td>WARN_NO_AVAILABLE_CHANNEL</td>
<td>103</td>
<td>No channel resources are available. Such as the server cannot allocate any channel resource.</td>
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
<td>The server rejected the request to open the channel. The server cannot process this request or the request is illegal.</td>
</tr>
<tr><td>WARN_REQUEST_DEFERRED</td>
<td>108</td>
<td>The user’s request is deferred.</td>
</tr>
</tbody>
</table>




