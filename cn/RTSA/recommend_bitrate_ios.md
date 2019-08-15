
---
title: 动态码率推荐
description: v1.1.1
platform: iOS,macOS
updatedAt: Thu Aug 15 2019 03:55:48 GMT+0800 (CST)
---
# 动态码率推荐
## 功能描述
为提升数据传输量和避免网络拥塞，Agora RTSA SDK 会根据网络带宽状况的变化建议开发者实时调整发送码率。

## 实现方法
有以下两种情况：
* 网络拥塞时，SDK 会触发 `rtcServiceRecommendDecreaseBitrateInChannel` 回调，建议某频道应该降低编码码率。收到此回调后，发送端按照回调中提示的值降低码率。
* 网络状况良好时，SDK 会触发 `rtcServiceRecommendIncreaseBitrateInChannel` 回调，建议某频道应该升高编码码率。收到此回调后，发送端按照回调中提示的值提高码率。

以下为示例代码，仅供参考：
~~~objective-c
uint32_t const VIDEO_TARGET_BPS_MAX = 1 << 23; // 8Mbps
uint32_t const VIDEO_TARGET_BPS_MIN = 1 << 16; // 64Kbps

- (uint32_t)checkTargetBps:(uint32_t)bps {
    // Make sure the target bps is valid
    if (bps > VIDEO_TARGET_BPS_MAX) {
        return VIDEO_TARGET_BPS_MAX;
    } else if (bps < VIDEO_TARGET_BPS_MAX) {
        return VIDEO_TARGET_BPS_MIN;
    } else {
        return bps;
    }
}

- (void)rtcServiceRecommendDecreaseBitrateInChannel:(NSString *)channelName count:(uint32_t)count {
    NSInteger targetBps = [self checkTargetBps:self.videoEncoder.targetBps - count];
    self.videoEncoder.targetBps = targetBps;
}

- (void)rtcServiceRecommendIncreaseBitrateInChannel:(NSString *)channelName count:(uint32_t)count {
    NSInteger targetBps = [self checkTargetBps:self.videoEncoder.targetBps + count];
    self.videoEncoder.targetBps = targetBps;
}
~~~

## API 参考
- [`rtcServiceRecommendDecreaseBitrateInChannel`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_oc/Protocols/AgoraRtcServiceEvents.html#//api/name/rtcServiceRecommendDecreaseBitrateInChannel:count:)
- [`rtcServiceRecommendIncreaseBitrateInChannel`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_oc/Protocols/AgoraRtcServiceEvents.html#//api/name/rtcServiceRecommendIncreaseBitrateInChannel:count:)

## 开发注意事项
* 这两个回调返回的是 bps，不要和 kbps 及 Bps 混淆。
* 这两个回调只表明网络好坏：`rtcServiceRecommendIncreaseBitrateInChannel` 表示网络可用带宽充足，可以增加编码码率；`rtcServiceRecommendDecreaseBitrateInChannel`表示网络可用带宽不够，应该减少编码码率。开发者应该自行控制码率上限和下限，确保码率不过大或过小。
