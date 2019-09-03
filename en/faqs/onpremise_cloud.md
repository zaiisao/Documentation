
---
title: What's the difference between on-premise recording and cloud recording?
description: 
platform: All Platforms,Linux
updatedAt: Tue Sep 03 2019 16:16:02 GMT+0800 (CST)
---
# What's the difference between on-premise recording and cloud recording?
For on-premise recording, you need to deploy the SDK on the Linux server but not for cloud recording, reducing development and maintenance costs. See the following table for details:

|                           | On-premise recording                                         | Cloud recording                                              |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Deployment method         | Private.                                                     | Public.                                                      |
| Usability                 | <li>You need to integrate the SDK. </li><li>You need to prepare server resources and deploy online.</li> | <li>Supports RESTful APIs.</li><li>You don't need to deploy the Linux server.</li> |
| Operation and maintenance | By yourself, on your on-premise server.                      | By Agora, on Agora's cloud server.                           |
| Scalability               | You need to prepare server resources and deploy online.      | Real time. No need for any server resources and deployment.  |
| Reliability               | Dependent on the implementation of your deployment.          | <li>Automatically saves files on the cloud server after a uploading failure.</li><li>Automatically resumes recording after a system failure or other issues.</li> |
