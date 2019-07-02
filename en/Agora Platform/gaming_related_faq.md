
---
title: Gaming-related Issues
description: 
platform: All Platforms
updatedAt: Tue Dec 25 2018 16:42:30 GMT+0800 (CST)
---
# Gaming-related Issues
## Gaming Sound Effects

### Before joining an audio channel, the gaming sound effects are muted through the system's mute button. After joining the channel, I can hear the gaming sound effects.

The volume is not 0 for calls in all communication applications. After joining an audio channel, the volume of the gaming sound effects is controlled by the call volume.

### I set the gaming sound effects at a certain system volume. After joining an audio channel, the volume of the gaming sound effects is higher than before.

The media and call volume controls are independent. After joining an audio channel, the gaming sound effects are controlled by the call volume. After leaving the audio channel, the gaming sound effects are controlled by the media volume. 

### On iOS, after leaving an audio channel during gameplay, the background music volume of the game is lower than before. This issue does not occur on Android.

Before joining the audio channel, Unity is used to play the gaming background music. The audio session's category is AVAudioSessionCategoryAmbient and the mode is AVAudioSessionModeDefault. The background music volume is controlled by the media volume.

After the joining the audio channel, the Agora SDK changes the audio session's category to AVAudioSessionCategoryPlayAndRecord and the mode to AVAudioSessionModeVoiceChat. The background music volume is controlled by the call volume.

The media and call volume controls are independent.

To work around this issue, when leaving an audio channel, you must set the AudioSession Category and Mode back to the settings before joining the channel.

### The gaming sound effects conflict with the voice after accessing the SDK.

This is expected behavior because the process of joining the channel is interrupted. You can set the gaming sound effects to be controlled by the media volume, but this may lead to other problems, such as echo.

### The background music of all players is mixed into a call in the Mahjong game.

This issue occurs if you do not use the Agora SDK to play the background music. To fix this issue, turn off the music and sound effects when joining an audio channel.

### The background music volume changes when joining the audio channel, and there is no background music when the leaveChannel method is called.

To fix this issue, call SetParameters (" {\ "che. Audio. Keep the audiosession \": true} ")  before calling the `joinChannel` method,

### The onAudioVolumeIndication callback returns a value of the volume between 0 and 255. What value indicates whether or not a user is speaking?

A value between 40 and 50 indicates that a user is speaking. This value range is subjective and you can adjust it accordingly.

### After joining an audio channel, the volume of the background music is lower than before.

This is because after joining a channel, the background music routes to the earpiece instead of the speakerphone. To fix this issue, call the `setEnablespeaker` method to ensure that the audio routes for the background music and call are the same.

### When the AMG SDK media system joins an audio channel after a host-in, the volume of the background music played by the media system cannot be adjusted.

To fix this issue, call the following methods before calling the `joinChannel` method:

**iOS** 

mRtcEngine.setParameters("{\"che.audio.use.remoteio\":true}");

**Android**

mRtcEngine.setParameters("{\"che.audio.stream_type\":3}");
mRtcEngine.setParameters("{\"che.audio.audioMode\":0}");

**Android and iOS **

mRtcEngine.setParameters("{\"che.audio.enable.aec\":true}");
mRtcEngine.setParameters("{\"che.audio.enable.agc\":true}");
mRtcEngine.setParameters("{\"che.audio.enable.ns\":true}");

### Why is there no stereo sound when using a Bluetooth headset?

Bluetooth terminology:

* A2DP (Advanced Audio Distribution Profile): A one-way, high-quality audio data transmission link used for playing stereo music.
* SCO (Synchronous Connection Oriented): A two-way transmission link for audio data, which only supports 8K and 16K mono-channel audio data, and used for voice transmissions.

Bluetooth headsets use two channels in the SCO link for audio playback and recording. However, Bluetooth headsets only support mono sound in SCO and stereo sound in A2DP.

If the Bluetooth headsets only support A2DP and not SCO, you cannot use them for voice calls.

## Device Compatibility

### Can I use Android and iPhone headsets interchangeably?
No, the layout of the left and right audio channels and microphone interfaces is different for iPhone and Android headsets.

