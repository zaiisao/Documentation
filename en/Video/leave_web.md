
---
title: Leave the Channel
description: 
platform: Web
updatedAt: Thu Dec 13 2018 08:43:45 GMT+0000 (UTC)
---
# Leave the Channel
When a call or live broadcast ends, use the Agora SDK to leave the channel.

## Implementation

Use the `client.leave`  method to remove the user from the current channel.

```javascript
client.leave(function () {
  console.log("Leavel channel successfully");
}, function (err) {
  console.log("Leave channel failed");
});
```

## Next Steps
You hava now integrated basic communication or live broadcast into your app, and can experience more advanced and complex functions following the articles listed under **Advanced Guide**.

If you encounter any problem integrating or using the Agora SDK, refer to the following documents or file a Ticket at [Agora Dashboard](https://dashboard.agora.io).

- [General Asked Questions](../../en/Agora%20Platform/general_questions.md)
- [Integration and Deployment](../../en/Agora%20Platform/general_questions.md)
- [Trouble Shooting](../../en/Agora%20Platform/general_questions.md)
