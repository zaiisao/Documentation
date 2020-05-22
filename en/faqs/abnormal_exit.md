
---
title: What should I do if my app crashes?
description: abnormal exit, abnormal
platform: All Platforms
updatedAt: Fri May 22 2020 15:14:09 GMT+0800 (CST)
---
# What should I do if my app crashes?
A recording session is not affected if an app with cloud recording integrated crashes. You can use the original App ID, channel name, and UID to call `acquire` and `start` to control the original recording instance.

<div class="alert info">If the recording times out and exits, calling <code>acquire</code> and <code>start</code> creates a new recording instance.</div>


