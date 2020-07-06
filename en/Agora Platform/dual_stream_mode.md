
---
title: dual-stream mode
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 12:47:09 GMT+0800 (CST)
---
# dual-stream mode
In the dual-stream mode, the SDK transmits a high-quality and a low-quality video stream from the sender. The high-quality video stream has a higher resolution and bitrate, and the low-quality video stream has a lower resolution and bitrate.

Subscribing to low-quality streams improves communication continuity because this reduces bandwidth consumption. Developers can choose to receive low-quality video streams when network condition are unreliable, or when multiple users publish streams.

The SDK sets the default video profile of the low-quality video stream based on that of the high-quality video stream.

<div class="alert info">See also:<li><a href="https://docs.agora.io/en/Interactive%20Broadcast/multi_user_video_android?platform=Android">Video for 7+ Users</a></li>
<li><a href="../../en/Agora%20Platform/terms.md">Stream fallback</a></li>
<li><a href="../../en/Agora%20Platform/terms.md">High-quality video stream</a></li>
<li><a href="../../en/Agora%20Platform/terms.md">Low-quality video stream</a></li>
</div>

<a href="../../en/Agora%20Platform/terms.md"><button>Back to glossary</button></a>
