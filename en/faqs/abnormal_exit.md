
---
title: What should I do if my app crashes?
description: abnormal exit, abnormal
platform: All Platforms
updatedAt: Fri May 22 2020 15:14:24 GMT+0800 (CST)
---
# What should I do if my app crashes?
If an app integrated with cloud recording crashes, the recording session is not affected. You can continue to use the original resource ID and recording ID to control the recording instance, such as to query recording status or stop recording.


<div class="alert note">Do not call <code>acquire</code> or <code>start</code> again with the same App ID, channel name and UID, which may cause an error or create a new recording instance.</div>
