
---
title: 动态码率推荐
description: v1.1.1
platform: Windows,Linux
updatedAt: Thu Aug 15 2019 03:55:52 GMT+0800 (CST)
---
# 动态码率推荐
## 功能描述
为提升数据传输量和避免网络拥塞，Agora RTSA SDK 会根据网络带宽状况的变化建议开发者实时调整发送码率。

## 实现方法
有以下两种情况：
* 网络拥塞时，SDK 会触发 `on_dec_bitrate` 回调，建议某频道应该降低编码码率。收到此回调后，发送端应该按照回调中提示的值降低码率。
* 网络状况良好时，SDK 会触发 `on_inc_bitrate` 回调，建议某频道应该升高编码码率。收到此回调后，发送端应该按照回调中提示的值提高码率。

以下为示例代码，仅供参考：
~~~c++
static constexpr int VIDEO_TARGET_BPS_MAX = 1 << 23; // 8Mbps
static constexpr int VIDEO_TARGET_BPS_MIN = 1 << 16; // 64Kbps

static constexpr int CheckTargetBps (int bps)
{
	// Make sure the target bps is valid
	if (bps > VIDEO_TARGET_BPS_MAX) {
		bps = VIDEO_TARGET_BPS_MAX;
	} else if (bps < VIDEO_TARGET_BPS_MAX) {
		bps = VIDEO_TARGET_BPS_MIN;
	}
	return bps;
}

static void OnDecBitrate (const char *channel, uint32_t bps)
{
	auto video_encoder = GetVideoEncoder (channel);
	const auto target_bps = CheckTargetBps (video_encoder.GetTargetBps () - bps);
	video_encoder.SetTargetBps (target_bps);
}

static void OnIncBitrate (const char *channel, uint32_t bps)
{
	auto video_encoder = GetVideoEncoder (channel);
	const auto target_bps = CheckTargetBps (video_encoder.GetTargetBps () + bps);
	video_encoder.SetTargetBps (target_bps);
}

static const agora_rtc_event_handler_t listener = {
		.on_dec_bitrate = OnDecBitrate,
		.on_inc_bitrate = OnIncBitrate,
		// ...
};
~~~

## API 参考
- [`on_dec_bitrate`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_c/structagora__rtc__event__handler__t.html#a7c5b33c2402058257cb5d0fbae017936)
- [`on_inc_bitrate`](https://docs.agora.io/cn/RTSA/API%20Reference/rtsa_c/structagora__rtc__event__handler__t.html#a59ca58445eb0f2067ea281274129b8ca)

## 开发注意事项
* 这两个回调返回的是 bps，不要和 kbps 及 Bps 混淆。
* 这两个回调只表明网络好坏：`on_inc_bitrate` 表示网络可用带宽充足，可以增加编码码率；`on_dec_bitrate `表示网络可用带宽不够，应该减少编码码率。开发者应该自行控制码率上限和下限，确保码率不过大或过小。
