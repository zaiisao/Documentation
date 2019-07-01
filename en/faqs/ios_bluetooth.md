
---
title: I connect a Bluetooth to a phone, but not all calls are answered through the Bluetooth headset.
description: iOS 蓝牙耳机相关问题
platform: iOS
updatedAt: Mon Jul 01 2019 15:11:06 GMT+0800 (CST)
---
# I connect a Bluetooth to a phone, but not all calls are answered through the Bluetooth headset.
iOS selects audio routes for phone and VoIP calls:

### Phone Call (Including Facetime and CallKit)

When a phone rings:

- If you press the answer button on the iPhone, the iPhone handset is used by default.
- If you press the answer button on the Bluetooth device, the Bluetooth headset is used by default.

You can change the audio route setting in Settings > General > Accessibility > Call Audio Routing. When you select the Bluetooth Headset option, all incoming calls will be answered through the headset even if you press the answer button on the iPhone.

During a phone call, you can switch between the Bluetooth Headset, Handset, or Speaker options in the call interface. 

## VoIP Call

When making a VoIP call, the default audio route is the one used by the last phone or VoIP call made after a Bluetooth device is connected. If no phone call was made since the Bluetooth device was connected, the VoIP calls will be answered through the Bluetooth headset.

During a VoIP call, users can change the audio route in the iPhone Control Center (swipe up from the bottom of the screen). Apps can call the setPreferredInput method to change the audio route.

## Other Settings

When users set the Bluetooth headset as the audio route on a VoIP app, the system switches to the Bluetooth headset mode accordingly.

Users can record calls with a Bluetooth speaker only when they make phone calls or use FaceTime or CallKit.

## Frequently Asked Questions

Q: When I am in a VoIP call with the Bluetooth headset and a phone call interrupts, why do I have to continue the VoIP call with the handset?
A: If you tap the answer button on your phone, the default audio route for the phone and VoIP calls switches to the iPhone handset.

If you tap the answer button on your Bluetooth headset, you can continue your call with the Bluetooth headset.

 

Q: My iPhone is connected to a Bluetooth headset. When I join a channel to make a VoIP call, why is the audio routed to the handset or speaker?
A: This is because the default route for the phone and VoIP calls is not the Bluetooth headset. Before making a VoIP call, you need to switch to the Bluetooth Headset mode in the Control Center (swipe up from the bottom of the screen).

 

Q: Why I can switch back to the Bluetooth Headset mode for music playback?
A: Music playback is different from VoIP calls.

 

Q: My iPhone is connected to a Bluetooth speaker. Why can't I use it to make a VoIP call?
A: VoIP calls cannot be recorded by a Bluetooth speaker unless the app uses the CallKit framework. Hence, users cannot answer VoIP calls with the Bluetooth speaker. In a live broadcast, the audience does not need to record their voice, so they can listen to the broadcast with the Bluetooth speaker.




