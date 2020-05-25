
---
title: 计费说明
description: 
platform: All Platforms
updatedAt: Mon May 25 2020 08:13:16 GMT+0800 (CST)
---
# 计费说明
## 概述



声网会按月对你使用的所有产品和服务分别计费，并收取总费用。本文介绍声网服务用量统计方式和实时消息产品的计费规则。如果你同时还使用了声网其他产品或服务，可查看对应的计费说明：






- [语音通话计费说明](https://docs.agora.io/cn/Voice/billing_audio?platform=All%20Platforms)
- [视频通话计费说明](https://docs.agora.io/cn/Video/billing_video?platform=All%20Platforms)
- [视频互动直播计费说明](https://docs.agora.io/cn/Interactive%20Broadcast/billing_interactive_broadcast?platform=All%20Platforms)
- [音频互动直播计费说明](https://docs.agora.io/cn/Audio%20Broadcast/billing_audio_broadcast?platform=All%20Platforms) 
- [本地服务端录制计费说明](https://docs.agora.io/cn/Recording/billing_recording?platform=All%20Platforms)
- [云端录制计费说明](https://docs.agora.io/cn/cloud-recording/billing_cloud_recording?platform=All%20Platforms)


账单、扣费及账户冻结等相关问题，详见[账单、扣费与账户冻结](https://docs.agora.io/cn/faq/billing_account)。


## 服务用量统计方式






声网根据 DAU (每日活跃用户数) 对 RTM 项目计费：每个项目中每个 `uid` (RTM) 或 `userAccount` (信令) 登录一次系统计为一次活跃，系统会对同一项目中每日内同一 `uid` 或 `userAccount` 的多次登录进行去重，以这种方式统计得到的日登录次数计为 RTM 项目的 DAU。<p>声网会将你的每个 [Agora 开发者账户](https://console.agora.io/)下每个项目的当月最高日活跃用户数 DAU 相加，根据相加得到的 DAU 数计算当月费用。</p><p><div class="alert note">为进一步推广 RTM 用量，声网决定自 2020 年 4 月 1 日至 2020 年 6 月 30 日对每月的最高日活跃用户数 DAU 减免 20000，根据减免后的当月最高 DAU 计费。</div></p>




## 产品计费表













| 当月最高日活跃用户数 DAU             | 报价（元/每 1000 人） |
| :--------------- | :---------------------- |
| &le; 1000         | 0                    |
| > 1000  | 100.00                   |


## 注意事项







### 当月最高日活跃用户 DAU 统计

- 同一 `uid` 或 `userAccount` 当日的多次登录只对当日 DAU 贡献一次，声网会对相同 `uid` 或 `userAccount` 的多次登录进行去重。
- 如果同一 [Agora 开发者账户](https://console.agora.io/)下存在多个 RTM 开发项目，当月最高 DAU 为各项目当月最高 DAU 的总和。



