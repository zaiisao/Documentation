
---
title: 语音通话计费说明
description: 语音通话计费说明
platform: All Platforms
updatedAt: Mon Jul 01 2019 15:21:08 GMT+0800 (CST)
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
    <td>通话费用 = 语音单价 x 总通话分钟数</td>
  </tr>
  <tr>
    <td>已启用录制</td>
    <td>通话费用 + 录制费用 = 语音单价 x（通话分钟数 + 录制分钟数）</td>
  </tr>
</table>

> 单价指的是每分钟的价格, 详见[价格方案](https://www.agora.io/cn/price/) 。
>
> 同一频道内只要有人开启录制（无论几人），录制均按照单路音频流计费。

## 示例 : 纯语音通信

本示例里，Agora 将收取你以下费用: 通话费 + 录制费

**通话费**

<table>
  <tr>
    <th>用户</th>
    <th>通话分钟数</th>
  </tr>
  <tr>
    <td>A</td>
    <td>30</td>
  </tr>
  <tr>
    <td>B</td>
    <td>40</td>
  </tr>
  <tr>
    <td>C</td>
    <td>20</td>
  </tr>
  <tr>
    <td>D</td>
    <td>15</td>
  </tr>
</table>

通话费 = 语音单价 x （30 + 40 + 20 + 15） 分钟

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
