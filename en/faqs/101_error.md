
---
title: Cloud recording 101 error
description: 
platform: All Platforms
updatedAt: Tue Nov 19 2019 10:56:16 GMT+0800 (CST)
---
# Cloud recording 101 error
The `"ErrorUint32":101` error in the log file is usually caused by a token error, such as in the following situations:

- A wrong token.
- An expired token.
- The Native/Web SDK uses a token while the cloud recording does not use a token.
- The cloud recording uses a token while the Native/Web SDK does not use a token.
