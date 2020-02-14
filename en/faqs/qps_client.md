
---
title: When mentioning qps, are you referring to the qps in the context of an RtmClient instance or in the context of one Agora RTM SDK?
description: 
platform: All Platforms
updatedAt: Fri Feb 14 2020 12:20:21 GMT+0800 (CST)
---
# When mentioning qps, are you referring to the qps in the context of an RtmClient instance or in the context of one Agora RTM SDK?
qps is short for Queries Per Second. When mentioning a qps limitation, we are referring to the qps of an API in the context of one single RtmClient instance, not in the context of one Agora RTM SDK. 

- On a Native platform, you can increase the qps limitation of an API by creating multiple RtmClient instances. 
- On a Web platform, we *do not recommend* increasing the qps limitation by creating multiple RtmClient instances. 
