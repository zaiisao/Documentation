
---
title: What's the difference between on-premise recording and cloud recording?
description: 
platform: All Platforms,Linux
updatedAt: Tue Sep 03 2019 16:16:28 GMT+0800 (CST)
---
# What's the difference between on-premise recording and cloud recording?
**Agora On-premise Recording** and **Agora Cloud Recording** are add-ons to record and save voice calls, video calls, and interactive broadcasts on your **Linux server** and your **cloud storage**.

Compared with Agora On-premise Recording, Agora Cloud Recording is more efficient and convenient as it does not require deploying powerful Linux servers, which largely reduces development and maintenance costs. You can directly start recording using RESTful APIs.

See the following table for details.

|                           | On-premise recording                                         | Cloud recording                                              |
| ------------------------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| Deployment method         | Deployed on your Linux server.                  | Deployed on Agora's cloud server.                                      |
| Usability                 | You have to prepare the server resources and deploy the SDK on your Linux server.  | You do not need to deploy the SDK on the Linux server and can directly use RESTful APIs to start recording. |
| Operation and maintenance | You need to maintain the recording service.                     | Agora maintains the recording service for you.                           |
| Scalability               | You need to scale to a more powerful server depending upon the concurrent usage/recording sessions.      | Agora scales up the recording service in real time at your request.  |
| Reliability               | The reliability of Agora On-premise Recording depends on your implementation.          | Agora Cloud Recording automatically saves files on Agora's cloud server for backup and automatically resumes recording after a system failure or other issues. |
