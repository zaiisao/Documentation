
---
title: Leave the Channel
description: 
platform: Web
updatedAt: Tue Oct 22 2019 06:18:48 GMT+0800 (CST)
---
# Leave the Channel
When a call or live broadcast ends, use the Agora SDK to leave the channel.

## Implementation

Use the `client.leave`  method to remove the user from the current channel.

```javascript
client.leave(function () {
  console.log("Leave channel successfully");
}, function (err) {
  console.log("Leave channel failed");
});
```

## Next Steps
You have integrated basic communication or live broadcast into your application. For advanced functions, see the sections under **Advanced Guide**.

If you encounter any problem integrating or using the Agora SDK, refer to the following sections or submit a ticket at [Agora Console](https://dashboard.agora.io).

- [General Questions](../../en/Agora%20Platform/general_questions.md)
- [Integration and Deployment](../../en/Agora%20Platform/general_questions.md)
- [Troubleshooting](../../en/Agora%20Platform/general_questions.md)
