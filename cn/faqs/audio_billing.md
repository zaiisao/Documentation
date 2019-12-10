
---
title: 语音通话计费说明
description: 语音通话计费说明
platform: All Platforms
updatedAt: Tue Dec 10 2019 17:26:11 GMT+0800 (CST)
---
# 语音通话计费说明
## 收费标准

通信按照分钟数和人数进行收费。扣除每月免费的 1 万分钟时长后，Agora 收取以下费用：

<table>
  <tr>
    <th>场景</th>
    <th>总费用</th>
  </tr>
  <tr>
    <td>未启用录制</td>
    <td>通话费用 = 语音单价 x 加入频道的总分钟数</td>
  </tr>
  <tr>
    <td>已启用录制</td>
    <td>通话费用 + 录制费用 = 语音单价 x（加入频道分钟数 + 录制分钟数）</td>
  </tr>
</table>

> 单价指的是每分钟的价格, 详见[价格方案](https://www.agora.io/cn/price/) 。
> 声网以秒计算每个用户的加入频道时间，计费时将所有用户的加入频道的总秒数求和并四舍五入折算成分钟数计费。
> 同一频道内只要有人开启录制（无论几人），录制均按照单路音频流计费。

## 示例 : 纯语音通信

本示例里，Agora 将收取你以下费用: 通话费 + 录制费

**通话费**

<table>
  <tr>
    <th>用户</th>
    <th>加入频道时间</th>
  </tr>
  <tr>
    <td>A</td>
    <td>90 秒</td>
  </tr>
  <tr>
    <td>B</td>
    <td>100 秒</td>
  </tr>
  <tr>
    <td>C</td>
    <td>200 秒</td>
  </tr>
  <tr>
    <td>D</td>
    <td>500 秒</td>
  </tr>
</table>

加入频道时间 = 90 + 100 + 200 + 500 = 890 秒 &asymp; 14.83 分钟 &asymp; 15 分钟
通话费 = 语音单价 x 加入频道时间 = 语音单价 x 15 分钟

**录制费**

<table>
  <tr>
    <th>用户名</th>
    <th>10 分钟</th>
    <th>20 分钟</th>
    <th>30 分钟</th>
    <th>40 分钟</th>
  </tr>
  <tr>
    <td>A</td>
    <td>录制</td>
    <td>录制</td>
    <td>录制</td>
    <td>录制</td>
  </tr>
  <tr>
    <td>B</td>
    <td></td>
    <td>录制</td>
    <td>录制</td>
    <td></td>
  </tr>
  <tr>
    <td>C</td>
    <td>录制</td>
    <td>录制</td>
    <td></td>
    <td></td>
  </tr>
  <tr>
    <td>D</td>
    <td>录制</td>
    <td></td>
    <td></td>
    <td></td>
  </tr>
</table>


录制费 = 语音单价 x 40 分钟。
