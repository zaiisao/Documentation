
---
title: Best Practices in Integrating Cloud Recording
description: 
platform: All Platforms
updatedAt: Mon Sep 07 2020 08:21:55 GMT+0800 (CST)
---
# Best Practices in Integrating Cloud Recording
To improve application robustness, Agora recommends that you do the following when integrating Cloud Recording RESTful APIs:

## Ensure the recording service starts successfully

Take the following steps to ensure that the recording service starts successfully:

1. Ensure that the `start` request is successful, that is, you receive a `sid` (recording ID) from the response. If the request fails, take measures according to the HTTP status code:

   - If the returned HTTP status code is always `40x`, check the parameter values in your request.
   - If the returned HTTP status code is `50x`, you can retry several times with the same parameters until you receive a `sid`. Agora recommends that you use a backoff strategy, for example, retry after 3, 6, and 9 seconds successively, to avoid exceeding the QPS limits. If you retry three times and still do not get a `sid`, retry with a new UID.

2. Five seconds after you receive a `sid`, use a backoff strategy to call `query`. Agora recommends that the interval between two `query` calls is shorter than `maxIdleTime`, which you set in the `start` call. If the `query` call succeeds, and `status` in `serverResponse` is `4` or `5`, it means the recording service starts successfully. Otherwise, you can deem the recording service as not having started, or quit halfway after starting.

   <div class="alert note">Agora recommends that you prepare a backup UID, which you can use when restarting the recording service, to avoid two identical UIDs kicking each other out of the channel. You can alternate between the backup UID and the original one.</div>

## Monitor service status during a recording

You can call `query` periodically to ensure that the recording is in progress and in a normal state. If `query` fails, take corresponding measures based on the status code:

- If the returned HTTP status code is always `40x`, check the parameter values in your request.
- If the returned HTTP status code is `404`, and the request parameters are confirmed to be correct, it means that the recording has either not started successfully, or the recording quit after starting. Agora recommends that you use a backoff strategy, for example, retry after 5, 10, and 15 seconds successively.
- If the returned HTTP status code is `50x`, the `query` request failed, but it is not clear whether the recording has quit. The `50x` error is rare. You can continue to use the backoff strategy (waiting for 5 seconds, 10 seconds, 15 seconds, or 30 seconds) to call `query`.

## Obtain the M3U8 file name

You can obtain the M3U8 file name by two means. One is by splicing according to the file naming rules. The other is by calling the `query` method. Agora recommends that you use splicing to obtain the M3U8 file name.

### Obtain file name by splicing

In composite recording mode, the format of the M3U8 file name is `<sid>_<cname>.m3u8`. Therefore, you can predict the M3U8 file name by splicing. See [Naming conventions](https://docs.agora.io/en/cloud-recording/cloud_recording_manage_files#合流模式) for details.

### Obtain file name via the `query` call

The M3U8 file name is generated after the first slice file is generated. Therefore, you should call `query` after the first slicing completes. See [Slicing](https://docs.agora.io/en/cloud-recording/cloud_recording_manage_files#slicing) for details.

In composite recording mode, call `query` 15 seconds after the cloud recording starts; in individual recording mode, call `query` one minute after the cloud recording starts. If the first `query` call fails, you can try again after one minute.

## Avoid frequent quits of the recording service

The default value of `maxIdleTime` in the `start` method is 30 seconds. If the host frequently goes online and offline, a brief `maxIdleTime` value causes the recording service to join and exit the channel frequently. For scenarios that require the recording service to be in the channel all the time, it is necessary to increase `maxIdleTime` in case the recording quits after a short idle time.

For example, if there is a fixed 5-minute break in each class, you can set `maxIdleTime` to 10 minutes to ensure uninterrupted recording of the entire class.
