
---
title: Leave the Channel
description: 
platform: Web
updatedAt: Thu Nov 01 2018 09:08:46 GMT+0000 (UTC)
---
# Leave the Channel
# Leave the Channel
Use the `client.leave`  method to remove the user from the current channel.

```javascript
client.leave(function () {
  console.log("Leavel channel successfully");
}, function (err) {
  console.log("Leave channel failed");
});
```
