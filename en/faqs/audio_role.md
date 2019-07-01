
---
title: How to prevent volume changes when the users switch their roles in a live broadcast
description: 
platform: All Platforms
updatedAt: Mon Jul 01 2019 15:10:16 GMT+0800 (CST)
---
# How to prevent volume changes when the users switch their roles in a live broadcast
To ensure better voice quality, the volume controls switch when the client role changes:
- The audience role uses the media volume control.
- The host role uses the call volume control. 

To avoid volume changes, you can set the media or call volume controls by setting `scenario` in `AudioProfile`. Select `CHATROOM_ENTERTAINMENT` for Android platforms and `GAME_STREAMING` for iOS platforms.
