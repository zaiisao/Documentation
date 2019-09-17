
---
title: Why do errors occur when calling the Stream.init method?
description: 
platform: Web
updatedAt: Tue Sep 17 2019 10:28:52 GMT+0800 (CST)
---
# Why do errors occur when calling the Stream.init method?
The following are common errors when initializing the stream:

- `NotAllowedError`: The user refuses to grant access to the video or audio resource.
- `NotFoundError`: Cannot find the specified media track. Ensure that your media device is working.
- `MEDIA_NOT_SUPPORT`: Use HTTPS for your web app.

See [`Stream.init`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/web/interfaces/agorartc.stream.html#init) for more errors.
