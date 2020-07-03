
---
title: 卡顿 (freeze)
description: 
platform: All Platforms
updatedAt: Fri Jul 03 2020 10:26:31 GMT+0800 (CST)
---
# 卡顿 (freeze)
卡顿是实时音视频传输过程中，因网络条件、设备性能受限等原因，引起的音频或视频播放断续、不流畅、甚至定格等现象。

Agora RTC SDK 会通过回调报告音频和视频的卡顿数据，方便开发者了解当前的传输质量，进行问题排查。开发者也可以通过声网开发者工具[水晶球](../../cn/Agora%20Platform/terms.md)了解卡顿情况。

其中，Agora RTC SDK 和水晶球判断发生卡顿的依据不同：

| 产品 | 视频卡顿 | 音频卡顿 |
| ---------------- | ---------------- | ---------------- |
| RTC SDK      | 通话过程中，视频帧率设置不低于 5 fps 时，连续渲染的两帧视频之间间隔超过 500 ms，即记为一次视频卡顿。      | 统计周期内，音频丢帧率达到 4%，即记为一次音频卡顿。      |
| 水晶球   | 通话过程中，视频卡顿达到 600 ms，即记为一次视频卡顿。| 通话过程中，音频卡顿达到 200 ms，即记为一次音频卡顿。|

<div class="alert info">相关链接：<a href="https://docs.agora.io/cn/Agora%20Platform/aa_data_insight?platform=All%20Platforms#quality">数据洞察质量概览</a>
</div>

<a href="../../cn/Agora%20Platform/terms.md"><button>返回术语库</button></a>
