
---
title: Test or Select a Media Device
description: 
platform: Windows
updatedAt: Wed Jul 17 2019 04:26:37 GMT+0800 (CST)
---
# Test or Select a Media Device
## Introduction

Agora supports the media device test and selection feature, allowing you to check if an audio device (a headset, microphone, or speaker) works or connects properly to the SD-RTN. For example, in an audio device test, if a user's recorded voice is replayed in 10 seconds, then the system's audio device works and is connected properly to the SD-RTN.

You can use the media device test and selection feature in the following scenarios:

- A host self-checks before starting a live broadcast.
- An online user checks if the media device works.

## Implementation

### Microphone Test

```C++
// Initialize the parameter object.
RtcEngineParameters rep(*lpRtcEngine);

// Enumerate all audio devices.
AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpRecordingDeviceCollection = (*lpDeviceManager)->enumerateRecordingDevices();

UINT lCount = (UINT) lpRecordingDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpRecordingDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// Select an audio capture device.
lpDeviceManager->setRecordingDevice(strDeviceID); // device ID chosen

// Implement the audio volume callback.
virtual void onAudioVolumeIndication(const AudioVolumeInfo* speakers, unsigned int speakerNumber, int totalVolume) {
        (void)speakers;
        (void)speakerNumber;
        (void)totalVolume;
    }

// Enable the audio volume callback.
rep.enableAudioVolumeIndication(1000, // Callback interval (ms)
	10 // Smoothness
	);

// Start the audio capture device test.
(*lpDeviceManager)->startRecordingDeviceTest(1000);

// Stop the audio capture device test.
(*lpDeviceManager)->stopRecordingDeviceTest();
```

### External Playback Device Test

```C++
// Initialize the parameter object.
RtcEngineParameters rep(*lpRtcEngine);

AAudioDeviceManager* lpDeviceManager = new AAudioDeviceManager(lpRtcEngine);
IAudioDeviceCollection *lpPlaybackDeviceCollection = (*lpDeviceManager)->enumeratePlaybackDevices();

UINT lCount = (UINT) lpPlaybackDeviceCollection->getCount();

CString strDeviceName;
CString strDeviceID;

for (UINT nIndex = 0; nIndex < lCount; nIndex++){
    int nRet = lpPlaybackDeviceCollection->getDevice(nIndex, strDeviceName, strDeviceID);
	...
}

// Select an audio playback device.
lpDeviceManager->setPlaybackDevice(strDeviceID); // device ID chosen

// Start the audio capture device test, and you will hear the audio from the external device.
// You do not need to call `enableAudioVolumeIndication`, because you can directly hear the audio.
(*lpDeviceManager)->startRecordingDeviceTest(1000);

// Stop the audio capture device test.
(*lpDeviceManager)->stopRecordingDeviceTest();
```

### API Reference

* [`enableAudioVolumeIndication`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_rtc_engine_parameters.html#a59ae67333fbc61a7002a46c809e2ec4f)
* [`enumerateRecordingDevices`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ea4f53d60dc91ea83960885f9ab77ee)
* [`setRecordingDevice`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a723941355030636cd7d183d53cc7ace7)
* [`enumeratePlaybackDevices`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#aa13c99d575d89e7ceeeb139be723b18a)
* [`setPlaybackDevice`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a1ee23eae83165a27bcbd88d80158b4f1)
* [`startRecordingDeviceTest`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a9e732d31f179a90d388998f5b86ebf06)
* [`stopRecordingDeviceTest`](https://docs.agora.io/en/Voice/API%20Reference/cpp/classagora_1_1rtc_1_1_i_audio_device_manager.html#a796e7b8a58eb303f18f04e1e9d12a94b)

## Considerations
- You need to ensure that the audio devices are not used by other applications when testing. 
- Do not call these tests with the `joinChannel` method.
