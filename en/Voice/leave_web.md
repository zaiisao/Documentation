
---
title: Leave the Channel
description: 
platform: Web
updatedAt: Fri Nov 02 2018 04:09:04 GMT+0000 (UTC)
---
# Leave the Channel
Use the `client.leave`  method to remove the user from the current channel.

```javascript
client.leave(function () {
  console.log("Leavel channel successfully");
}, function (err) {
  console.log("Leave channel failed");
});
```
