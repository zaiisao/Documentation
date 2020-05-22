
---
title: 应用崩溃后我该怎么办？
description: 
platform: All Platforms
updatedAt: Fri May 22 2020 14:18:18 GMT+0800 (CST)
---
# 应用崩溃后我该怎么办？
集成云端录制的应用崩溃，不会影响录制进程。如需控制原来的录制实例，你需要使用和之前录制相同的 App ID、频道名以及 UID，再次调用 `acquire` 和 `start` 方法。

<div class="alert info">如果原来的录制已经超时退出，则再次调用 <code>acquire</code> 和 <code>start</code> 会创建新的录制实例。</div>
