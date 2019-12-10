
---
title: 计费说明
description: 
platform: All Platforms
updatedAt: Tue Dec 10 2019 00:24:53 GMT+0800 (CST)
---
# 计费说明
## 概述



声网会按月对你使用的所有产品或服务分别计费，并收取总费用。本文介绍声网服务用量统计方式和语音通话产品的计费规则。如果你同时还使用了声网其他产品或服务，可查看对应的计费说明：


- [本地服务端录制计费说明](https://docs.agora.io/cn/Recording/billing_recording?platform=All%20Platforms)
- [云端录制计费说明](https://docs.agora.io/cn/cloud-recording/billing_cloud_recording?platform=All%20Platforms)



账单、扣费及账户冻结等相关问题，详见[账单、扣费与账户冻结](https://docs.agora.io/cn/faq/billing_account)。



## 10000 分钟免费时长声明

声网会给予每个 [App ID](https://console.agora.io/) 每个月1 万分钟的免费时长，按照以下顺序从总分钟数扣除：

1. 音频分钟数
2. 本地服务端录制音频分钟数
3. 云端录制音频分钟数
4. 小程序音频分钟数
5. 高清视频分钟数 HD
6. 本地录制高清视频分钟数 HD
7. 小程序录制高清视频分钟数 HD
8. 云端录制高清视频分钟数 HD
9. 超清视频分钟数 HD+
10. 本地服务端录制超清视频分钟数 HD+
11. 云端录制超清视频分钟数 HD+

如果实际使用分钟数未超过 10000 分钟，则本月免费；如果 1 万分钟的免费额度扣完，则对剩余的分钟数扣取相应服务费用。

<div class="alert note">每月剩余分钟数会清零。</div>

## 服务用量统计方式


声网会按月统计你的每个 [App ID](https://console.agora.io/) 对应项目使用的[音频分钟数](#amin)。







### <a name="amin"></a>音频分钟数 

用户在 RTC 频道内的时间扣除订阅视频流的分钟数后所得剩余时间，无论是否订阅音频流都记为音频分钟数。


<div class="alert note"><li>同时订阅了多路音频流的同一用户的音频分钟数不会被叠加。</li><li>音频分钟数的定义和计算方法也适用于小程序。</li><li>音频分钟数的计费方案详见<a href="#billing">产品计费表</a>。</li></div>






## 产品计费表



| 服务<a name="billing"></a>       | 报价（元/每 1000 分钟） |
| :--------- | :---------------------- |
| 音频       | 7.00                    |
| 小程序音频 | 10.00                   |











## 常见问题


<details>
	<summary><font color="#3ab7f8">问：我看到的音频服务使用时间是针对某个用户的吗？</font></summary>

不是的。你看到的音频分钟数是你的这个 App ID 下的所有用户的音频服务使用时间的汇总。换言之，我们提供的音频分钟数既不是某个用户的分钟数，也不是某一个频道内的所有用户的分钟数，而是该 App ID 下所有频道内的所有用户的分钟数的总和。

</details>







