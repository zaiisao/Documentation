
---
title: RESTful API
description: acquire, start, updateLayout, query, stop， appid, cname, uid, clientRequest, resourceExpiredHour， appid, resourceid, mode, cname, uid, clientRequest, token, recordingConfig, streamTypes, decryptionMode, secret, channelType, audioProfile, videoStreamType, maxIdleTime,transcodingConfig, width, height, fps, bitrate, maxResolutionUid, mixedVideoLayout, backgroundColor, layoutConfig, ui, x_axis, width, height, alpha, render_mode, subscribeVideoUids,subscribeAudioUids, subscribeUidGroup, storageConfig, vendor, region, bucket, accessKey, secretKey, fileNamePrefix， resourceId, sid, serverResponse, fileListMode, fileList, filename, trackType, uid, mixedAllUser, isPlayable, sliceStartTime, status, sliceStartTime 
platform: All Platforms
updatedAt: Tue Jun 09 2020 11:32:59 GMT+0800 (CST)
---
# RESTful API
Visit our interactive API documentation at [Cloud Recording RESTful API](https://docs.agora.io/en/cloud-recording/restfulapi). This documentation contains detailed help for each Cloud Recording RESTful API and its parameters, and provides the **Try it out** function which allows you to send RESTful API requests and receive responses directly on the web page.

## API Overview

| API                                                          | Description                                                  |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`acquire`](https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/acquire) | Before starting a cloud recording, you need to call this method to get a resource ID. |
| [`start`](https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/start) | After getting a resource ID, call this method to start cloud recording. |
| [`updateLayout`](https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/updateLayout) | During a recording, you can call this method to update the video mixing layout multiple times. |
| [`query`](https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/query) | After you start a recording, you can call `query` to check its status. |
| [`stop`](https://docs.agora.io/en/cloud-recording/restfulapi/#/Cloud%20Recording/stop) | When a recording finishes, call this method to leave the channel and stop recording. To use Agora Cloud Recording again, you need to call the `acquire` method for a new resource ID. |


