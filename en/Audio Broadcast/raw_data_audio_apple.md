
---
title: Raw Audio Data
description: 
platform: iOS,macOS
updatedAt: Fri Nov 06 2020 04:29:20 GMT+0800 (CST)
---
# Raw Audio Data
## Introduction

During real-time communications, you can pre- and post-process the audio and video data and modify them for desired playback effects

The Native SDK uses the `IAudioFrameObserver` and `IVideoFrameObserver` class to provide raw data functions. You can pre-process the data before sending it to the encoder and modify the captured video frames and voice signals. You can also post-process the data after sending it to the decoder and modify the received video frames and voice signals.

This article tells how to use raw audio data with the `IAudioFrameObserver` class.

## Sample project
Agora provides the open-source sample project that implements the raw audio data function on GitHub:

- iOS: [RawMediaData](../../en/Audio%20Broadcast/raw_data_audio_apple.md)
- macOS: [RawMediaData](../../en/Audio%20Broadcast/raw_data_audio_apple.md)

You can download them to try out this function and view the source code.

## Implementation

Before proceeding, ensure that you have implemented the basic real-time communication functions in your project.

Follow these steps to implement the raw data functions in your project:

1. Call the `registerAudioFrameObserver` method to register a audio observer object before joining the channel. You need to implement an `IAudioFrameObserver` class in this method.
2. After you successfully register the observer object, the SDK triggers the  `onRecordAudioFrame`, `onPlaybackAudioFrame`, `onPlaybackAudioFrameBeforeMixing`, or `onMixedAudioFrame`  callback to send the raw audio data at the set time interval.
3. After acquiring the raw data, you can process the captured raw data based on your scenario and send the processed data to the SDK via the callbacks mentioned in step 2.

<div class="alert note">The Agora SDK provides only C++ methods and callbacks for implementing the raw audio data function. Therefore, to implement the preceding steps, you need to call C++ methods on iOS or macOS.
	<ul><li>Code logics where both Objective-C and C++ methods are called should be implemented in an <code>.mm</code> file.</li><li>Ensure that you import the C++ header file at the beginning of the <code>.mm</code> file.</li><li>Before calling <code>registerAudioFrameObserver</code>, you need to call <code>getNativeHandler</code> in order to receive C++ callbacks.</li></ul>
</div>

### Data transfer

The following diagram shows the data transfer with the `IAudioFrameObserver` class.

![](https://web-cdn.agora.io/docs-files/1604635727525)

With `onRecordAudioFrame`, `onPlaybackAudioFrame`, `onPlaybackAudioFrameBeforeMixing`, or `onMixedAudioFrame` , you can:

- Get raw audio frames.
- Process the raw frames and return to the SDK.


### API call sequence

The following diagram shows how to implement the raw data functions in your project:

![](https://web-cdn.agora.io/docs-files/1569210004744)

### Sample code

In addition to the API call sequence diagram, you can refer to the following code samples.

**1. Initialize AgoraRtcEngineKit**

Call `sharedEngineWithConfig` to initialize AgoraRtcEngineKit.

```swift
// Swift
// Initialize AgoraRtcEngineKit
let config = AgoraRtcEngineConfig()
agoraKit = AgoraRtcEngineKit.sharedEngine(with: config, delegate: self)
```

**2. Register an audio frame observer**

The Agora SDK provides only C++ methods and callbacks for implementing the raw audio data function. Therefore, you need to call C++ methods on iOS or macOS to register an audio frame observer.

<div class="alert note">The following code should be implemented in the <code>.mm</code> file.</div>

```C++
// Import the C++ header file
#import <AgoraRtcKit/IAgoraMediaEngine.h>
#import <AgoraRtcKit/IAgoraRtcEngine.h>
  
- (void)registerAudioRawDataObserver:(ObserverAudioType)observerType {
    // Gets the C++ event handler of the RTC Native SDK
    agora::rtc::IRtcEngine* rtc_engine = (agora::rtc::IRtcEngine*)self.agoraKit.getNativeHandle;
    // Creates an IMediaEngine instance
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    // Ensure that you call queryInterface in IMediaEngine to set the agora::AGORA_IID_MEDIA_ENGINE interface, or you cannot implement registerAudioFrameObserver
    mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
      
    NSInteger oldValue = self.observerAudioType;
    self.observerAudioType |= observerType;
      
    if (mediaEngine && oldValue == 0)
    {
        // Register an audio frame observer
        mediaEngine->registerAudioFrameObserver(&s_audioFrameObserver);
        s_audioFrameObserver.mediaDataPlugin = self;
    }
}
```

**3. Set the audio frame capture parameters**

If you want to modify the audio sampling rate, use mode, number of channels, or sampling interval of the captured audio data, you can call the following methods:

```swift
// Swift
// Sets the audio data format returned in onRecordAudioFrame
agoraKit.setRecordingAudioFrameParametersWithSampleRate(44100, channel: 1, mode: .readWrite, samplesPerCall: 4410)
// Sets the audio data format returned in onMixedAudioFrame
agoraKit.setMixedAudioFrameParametersWithSampleRate(44100, samplesPerCall: 4410)
// Sets the audio data format returned in onPlaybackAudioFrame
agoraKit.setPlaybackAudioFrameParametersWithSampleRate(44100, channel: 1, mode: .readWrite, samplesPerCall: 4410)
```

**4. Join a channel**

Call `joinChannelByToken` to join an RTC channel.

```swift
// Swift
agoraKit.joinChannel(byToken: nil, channelId: channelName, info: nil, uid: 0) {[unowned self] (channel, uid, elapsed)}
```

**5. Receive the captured audio data**

After joining a channel, you can receive the captured audio data from the callbacks in the `IAudioFrameObserver` class. You can also use these callbacks to send the processed audio data back to the SDK.

```swift
// Swift
// Gets the raw audio data of the local user, and sends the data back to the SDK after processing
func mediaDataPlugin(_mediaDataPlugin: AgoraMediaDataPlugin, didRecord audioRawData: AgoraAudioRawDate) -> AgoraAudioRawData {
  return audioRawData
}
  
// Gets the raw audio data of all remote users, and sends the data back to the SDK after processing
func mediaDataPlugin(_mediaDataPlugin: AgoraMediaDataPlugin, willPlaybackAudioRawData audioRawData: AgoraRawData) -> AgoraAudioRawData {
  return audioRawData
}
  
// Gets the raw audio data of a specified remote user, and sends the data back to the SDK after processing
func mediaDataPlugin(_mediaDataPlugin: AgoraMediaDataPlugin, willPlaybackBeforeMixing audioRawData: AgoraAudioRawData, ofUid uid: uint) -> AgoraAudioRawData {
  return audioRawData
}
  
// Gets the raw audio data of the local user and all remote users, and sends the data back to the SDK after processing
func mediaDataPlugin(_mediaDataPlugin: AgoraMediaDataPlugin, didMixedAudioRawData audioRawData: AgoraAudioRawData) -> AgoraAudioRawData {
  return audioRawData
}
```

Call the C++ methods in the `.mm` file to implement the callbacks.

```swift
// Swift
class AgoraMediaDataPluginAudioFrameObserver : public agora::media::IAudioFrameObserver
{
public:
    AgoraMediaDataPlugin *mediaDataPlugin;
  
    // Defines the format of the raw audio data
    AgoraAudioRawData* getAudioRawDataWithAudioFrame(AudioFrame& audioFrame)
    {
        AgoraAudioRawData *data = [[AgoraAudioRawData alloc] init];
        data.samples = audioFrame.samples;
        data.bytesPerSample = audioFrame.bytesPerSample;
        data.channels = audioFrame.channels;
        data.samplesPerSec = audioFrame.samplesPerSec;
        data.renderTimeMs = audioFrame.renderTimeMs;
        data.buffer = (char *)audioFrame.buffer;
        data.bufferSize = audioFrame.samples * audioFrame.bytesPerSample;
        return data;
    }
      
    // Defines the format of the processed audio data
    void modifiedAudioFrameWithNewAudioRawData(AudioFrame& audioFrame, AgoraAudioRawData *audioRawData)
    {
        audioFrame.samples = audioRawData.samples;
        audioFrame.bytesPerSample = audioRawData.bytesPerSample;
        audioFrame.channels = audioRawData.channels;
        audioFrame.samplesPerSec = audioRawData.samplesPerSec;
        audioFrame.renderTimeMs = audioRawData.renderTimeMs;
    }
      
    // Receives the raw audio data of the local user from onRecordAudioFrame
    virtual bool onRecordAudioFrame(AudioFrame& audioFrame) override
    {
          
        if (!mediaDataPlugin && ((mediaDataPlugin.observerAudioType >> 0) == 0)) return true;
        @autoreleasepool {
            if ([mediaDataPlugin.audioDelegate respondsToSelector:@selector(mediaDataPlugin:didRecordAudioRawData:)]) {
                AgoraAudioRawData *data = getAudioRawDataWithAudioFrame(audioFrame);
                AgoraAudioRawData *newData = [mediaDataPlugin.audioDelegate mediaDataPlugin:mediaDataPlugin didRecordAudioRawData:data];
                modifiedAudioFrameWithNewAudioRawData(audioFrame, newData);
            }
        }
  
        // Sets the return value as true, meaning to send the data back to the SDK
        return true;
    }
      
    // Receives the raw audio data of all remote users from onPlaybackAudioFrame
    virtual bool onPlaybackAudioFrame(AudioFrame& audioFrame) override
    {
          
        if (!mediaDataPlugin && ((mediaDataPlugin.observerAudioType >> 1) == 0)) return true;
        @autoreleasepool {
            if ([mediaDataPlugin.audioDelegate respondsToSelector:@selector(mediaDataPlugin:willPlaybackAudioRawData:)]) {
                AgoraAudioRawData *data = getAudioRawDataWithAudioFrame(audioFrame);
                AgoraAudioRawData *newData = [mediaDataPlugin.audioDelegate mediaDataPlugin:mediaDataPlugin willPlaybackAudioRawData:data];
                modifiedAudioFrameWithNewAudioRawData(audioFrame, newData);
            }
        }
        // Sets the return value as true, meaning to send the data back to the SDK
        return true;
    }
  
    // Receives the raw audio data of all remote users from onPlaybackAudioFrameBeforeMixing  
    virtual bool onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame) override
    {
          
        if (!mediaDataPlugin && ((mediaDataPlugin.observerAudioType >> 2) == 0)) return true;
        @autoreleasepool {
            if ([mediaDataPlugin.audioDelegate respondsToSelector:@selector(mediaDataPlugin:willPlaybackBeforeMixingAudioRawData:ofUid:)]) {
                AgoraAudioRawData *data = getAudioRawDataWithAudioFrame(audioFrame);
                AgoraAudioRawData *newData = [mediaDataPlugin.audioDelegate mediaDataPlugin:mediaDataPlugin willPlaybackBeforeMixingAudioRawData:data ofUid:uid];
                modifiedAudioFrameWithNewAudioRawData(audioFrame, newData);
            }
        }
        // Sets the return value as true, meaning to send the data back to the SDK
        return true;
    }
      
    // Receives the raw audio data of the local user and all remote users from onMixedAudioFrame
    virtual bool onMixedAudioFrame(AudioFrame& audioFrame) override
    {
          
        if (!mediaDataPlugin && ((mediaDataPlugin.observerAudioType >> 3) == 0)) return true;
        @autoreleasepool {
            if ([mediaDataPlugin.audioDelegate respondsToSelector:@selector(mediaDataPlugin:didMixedAudioRawData:)]) {
                AgoraAudioRawData *data = getAudioRawDataWithAudioFrame(audioFrame);
                AgoraAudioRawData *newData = [mediaDataPlugin.audioDelegate mediaDataPlugin:mediaDataPlugin didMixedAudioRawData:data];
                modifiedAudioFrameWithNewAudioRawData(audioFrame, newData);
            }
        }
        // Sets the return value as true, meaning to send the data back to the SDK
        return true;
    }
};
```

**6. Stop registering the audio frame observer**

Call `registerAudioFrameObserver(NULL)` to stop registering the audio frame observer.

<div class="alert note">The following code should be implemented in the <code>.mm</code> file.</div>
	
```C++
- (void)deregisterAudioRawDataObserver:(ObserverAudioType)observerType {
    agora::rtc::IRtcEngine* rtc_engine = (agora::rtc::IRtcEngine*)self.agoraKit.getNativeHandle;
    agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
    mediaEngine.queryInterface(rtc_engine, agora::AGORA_IID_MEDIA_ENGINE);
  
    self.observerAudioType ^= observerType;
  
    if (mediaEngine && self.observerAudioType == 0)
    {
        mediaEngine->registerAudioFrameObserver(NULL);
        s_audioFrameObserver.mediaDataPlugin = nil;
    }
}
```
	
### API Reference

- [registerAudioFrameObserver](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae46ca0d20789787aaab2fb268a524100)
- [setRecordingAudioFrameParameters](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a2c4717760b5fbf1bb8c1a3c16ca67fe5)
- [setPlaybackAudioFrameParameters](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#aa5f2f6eb3db5acaaf8c40818d90694f1)
- [setMixedAudioFrameParameters](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a520ebcda51b5eb488339f3a12dfb8013)
- [onRecordAudioFrame](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ac6ab0c792420daf929fed78f9d39f728)
- [onPlaybackAudioFrame](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#aefc7f9cb0d1fcbc787775588bc849bac)
- [onPlaybackAudioFrameBeforeMixing](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#ae04d85a65eefec5e7c1e0477bcaa067c)
- [onMixedAudioFrame](https://docs.agora.io/en/Audio%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_audio_frame_observer.html#a78d095cbd0b8ee04f657430bb6de8100)


## Considerations

You need to call C++ methods on iOS or macOS to implement these functions.

- Code logics where both Objective-C and C++ methods are called should be implemented in an `.mm` file.
- Ensure that you add the following lines at the beginning of the .mm file to import the C++ header file:

	```
#import <AgoraRtcKit/IAgoraMediaEngine.h>
#import <AgoraRtcKit/IAgoraRtcEngine.h>
	```

- Before calling registerAudioFrameObserver, you need to call getNativeHandler in order to receive C++ callbacks.

	```
agora::rtc::IRtcEngine* rtc_engine = (agora::rtc::IRtcEngine*)self.agoraKit.getNativeHandle;
	```

You can find the complete code for calling C++ methods on iOS or macOS on [AgoraMediaDataPlugin.mm](https://github.com/AgoraIO/API-Examples/blob/master/iOS/APIExample/Common/RawDataApi/AgoraMediaDataPlugin.mm).
