
---
title: 云端录制 RESTful API
description: acquire, start, updateLayout, query, stop， appid, cname, uid, clientRequest, resourceExpiredHour， appid, resourceid, mode, cname, uid, clientRequest, token, recordingConfig, streamTypes, decryptionMode, secret, channelType, audioProfile, videoStreamType, maxIdleTime,transcodingConfig, width, height, fps, bitrate, maxResolutionUid, mixedVideoLayout, backgroundColor, layoutConfig, ui, x_axis, width, height, alpha, render_mode, subscribeVideoUids,subscribeAudioUids, subscribeUidGroup, storageConfig, vendor, region, bucket, accessKey, secretKey, fileNamePrefix， resourceId, sid, serverResponse, fileListMode, fileList, filename, trackType, uid, mixedAllUser, isPlayable, sliceStartTime, status, sliceStartTime 
platform: All Platforms
updatedAt: Tue Jun 09 2020 11:30:30 GMT+0800 (CST)
---
# 云端录制 RESTful API
点击[云端录制 RESTful API](https://docs.agora.io/cn/cloud-recording/restfulapi) 查看我们全新的交互式 API  文档。该互动式文档包含各方法和参数的详细介绍，并提供 **Try it out** 功能，使你在文档页内即能进行 RESTful API 的调试。

## API 概览

| 方法                                                         | 解释                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| [`acquire`](https://docs.agora.io/cn/cloud-recording/restfulapi/#/%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6/acquire) | 在开始云端录制之前，你需要调用该方法获取一个 resource ID。   |
| [`start`](https://docs.agora.io/cn/cloud-recording/restfulapi/#/%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6/start) | 获取 resource ID 后，调用该方法开始云端录制。                |
| [`updateLayout`](https://docs.agora.io/cn/cloud-recording/restfulapi/#/%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6/updateLayout) | 在录制过程中，可以随时调用该方法更新合流布局的设置。         |
| [`query`](https://docs.agora.io/cn/cloud-recording/restfulapi/#/%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6/query) | 开始录制后，你可以调用 `query`查询录制状态。                  |
| [`stop`](https://docs.agora.io/cn/cloud-recording/restfulapi/#/%E4%BA%91%E7%AB%AF%E5%BD%95%E5%88%B6/stop) | 录制完成后，调用该方法离开频道，停止录制。录制停止后如需再次录制，必须再调用 `acquire` 方法请求一个新的 resource ID。 |



