
---
title: Custom Audio Source and Renderer
description: 
platform: Windows
updatedAt: Mon Jul 06 2020 10:35:05 GMT+0800 (CST)
---
# Custom Audio Source and Renderer
## Introduction

Before reading this article, you need to undersand two concepts: audio source and audio sink. Audio source is where audio frames come from, such as a microphone and a recorder. Audio sink is where audio frames go, such as a speaker and a earpiece.

By default, the Agora SDK uses default audio and video modules for capturing and rendering in real-time communications. 

However, the default modules might not meet your development requirements, such as in the following scenarios:

- Your app has its own audio or video module.
- You want to use a non-camera source, such as recorded screen data.
- You need to process the captured video with a pre-processing library for functions such as image enhancement.
- You need flexible device resource allocation to avoid conflicts with other services.

This article tells you how to use the Agora Native SDK to customize the audio source and sink.

## Implementation

Before customizing the audio source or sink, ensure that you implement the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Interactive%20Broadcast/start_call_windows.md) or [Start Live Interactive Streaming](../../en/Interactive%20Broadcast/start_live_windows.md).

### Custom audio source

Refer to the following steps to customize the audio source in your project:

1. Call the `setExternalAudioSource` method to enable the external audio source before joining a channel.
2. Record and process the audio data on your own.
3. Send the audio data back to the SDK using the `pushExternalAudioFrame` method.

**API call sequence**

Refer to the following diagram to customize the audio source in your project.

![](https://web-cdn.agora.io/docs-files/1569383958150)

**Sample code**

```cpp
// Preparation. Implement the audio capture and audio playback queue to store the captured data or data for playback.
CAudioPlayPackageQueue	*CAudioPlayPackageQueue::m_lpAudioPackageQueue = NULL;

CAudioPlayPackageQueue::CAudioPlayPackageQueue()
{
  m_bufQueue.Create(32, 8192);
  m_nPackageSize = 8192;
}

CAudioPlayPackageQueue::~CAudioPlayPackageQueue()
{
  m_bufQueue.FreeAllBusyBlock();
  m_bufQueue.Close();
}

CAudioPlayPackageQueue *CAudioPlayPackageQueue::GetInstance()
{
  if (m_lpAudioPackageQueue == NULL)
    m_lpAudioPackageQueue = new CAudioPlayPackageQueue();

  return m_lpAudioPackageQueue;
}

void CAudioPlayPackageQueue::CloseInstance()
{
  if (m_lpAudioPackageQueue == NULL)
    return;

  delete m_lpAudioPackageQueue;
  m_lpAudioPackageQueue = NULL;
}

void CAudioPlayPackageQueue::SetAudioPackageSize(SIZE_T nPackageSize)
{
  _ASSERT(nPackageSize > 0 && nPackageSize <= m_bufQueue.GetBytesPreUnit());

  if (nPackageSize == 0 || nPackageSize > m_bufQueue.GetBytesPreUnit())
    return;

  m_nPackageSize = nPackageSize;
}

void CAudioPlayPackageQueue::SetAudioFormat(const WAVEFORMATEX *lpWaveInfo)
{
  memcpy_s(&m_waveFormat, sizeof(WAVEFORMATEX), lpWaveInfo, sizeof(WAVEFORMATEX));
}

void CAudioPlayPackageQueue::GetAudioFormat(WAVEFORMATEX *lpWaveInfo)
{
  memcpy_s(lpWaveInfo, sizeof(WAVEFORMATEX), &m_waveFormat, sizeof(WAVEFORMATEX));
}

BOOL CAudioPlayPackageQueue::PushAudioPackage(LPCVOID lpAudioPackage, SIZE_T nPackageSize)
{
  if (m_bufQueue.GetFreeCount() == 0)
    m_bufQueue.FreeBusyHead(NULL, 0);

  LPVOID lpBuffer = m_bufQueue.AllocBuffer(FALSE);
  if (lpBuffer == NULL)
    return FALSE;

  _ASSERT(m_bufQueue.GetBytesPreUnit() >= nPackageSize);

  memcpy_s(lpBuffer, m_bufQueue.GetBytesPreUnit(), lpAudioPackage, nPackageSize);

  return TRUE;
}

BOOL CAudioPlayPackageQueue::PopAudioPackage(LPVOID lpAudioPackage, SIZE_T *nPackageSize)
{
  _ASSERT(nPackageSize != NULL);

  if (nPackageSize == NULL)
    return FALSE;

  if (m_bufQueue.GetFreeCount() == 0)
    return FALSE;

  if (*nPackageSize < m_nPackageSize) {
    *nPackageSize = m_nPackageSize;
    return FALSE;
  }

  *nPackageSize = m_nPackageSize;
  m_bufQueue.FreeBusyHead(lpAudioPackage, m_nPackageSize);

  return TRUE;
}

// Implement an audio observer for the external audio source.
CExternalAudioFrameObserver::CExternalAudioFrameObserver()
{
}

CExternalAudioFrameObserver::~CExternalAudioFrameObserver()
{
}

bool CExternalAudioFrameObserver::onRecordAudioFrame(AudioFrame& audioFrame)
{
  SIZE_T nSize = audioFrame.channels*audioFrame.samples * 2;
  CAudioCapturePackageQueue::GetInstance()->PopAudioPackage(audioFrame.buffer, &nSize);

  return true;
}

bool CExternalAudioFrameObserver::onPlaybackAudioFrame(AudioFrame& audioFrame)
{
  SIZE_T nSize = audioFrame.channels*audioFrame.samples * 2;
  CAudioPlayPackageQueue::GetInstance()->PushAudioPackage(audioFrame.buffer, nSize);

  return true;
}

bool CExternalAudioFrameObserver::onMixedAudioFrame(AudioFrame& audioFrame)
{
  return true;
}

bool CExternalAudioFrameObserver::onPlaybackAudioFrameBeforeMixing(unsigned int uid, AudioFrame& audioFrame)
{
  return true;
}

// Enable the external audio source mode and register the audio observer. Use the observer to pass the data from the external source to the engine, and then from the engine to the app.
int nRet = rtcEngine.setExternalAudioSource(true, nSampleRate, nChannels);

agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
mediaEngine.queryInterface(lpAgoraEngine, agora::AGORA_IID_MEDIA_ENGINE);

mediaEngine->registerAudioFrameObserver(lpExternalAudioFrameObserver);

// Start pushing the data into the engine and retrieve the data from the engine. You may need to maintain a process loop.

CAudioCapturePackageQueue *lpBufferQueue = CAudioCapturePackageQueue::GetInstance();

IAudioFrameObserver::AudioFrame frame;
frame.bytesPerSample = gWaveFormatEx.wBitsPerSample / 8;
frame.channels = gWaveFormatEx.nChannels;
frame.renderTimeMs = 10;
frame.samples = gWaveFormatEx.nSamplesPerSec / 100;
frame.samplesPerSec = gWaveFormatEx.nSamplesPerSec;
frame.type = IAudioFrameObserver::AUDIO_FRAME_TYPE::FRAME_TYPE_PCM16;

  do {
    if (::WaitForSingleObject(lpParam->hExitEvent, 0) == WAIT_OBJECT_0)
    break;

    nAudioBufferSize = 8192;

    if (!lpBufferQueue->PopAudioPackage(lpAudioData, &nAudioBufferSize)) {
      continue;
    }

    frame.buffer = lpAudioData;

    mediaEngine->pushAudioFrame(MEDIA_SOURCE_TYPE::AUDIO_RECORDING_SOURCE, &frame, true);
  } while (TRUE);

// Disable the external audio source mode.
int nRet = rtcEngine.setExternalAudioSource(false, nSampleRate, nChannels);

mediaEngine->registerAudioFrameObserver(NULL);
```

**API reference**

- [`setExternalAudioSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a1dfb925d8ba1a60ca1d9ca04a4d7d65f)
- [`pushAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a2d3bb76cbc7008960166bb292a193616)

### Custom audio sink

Refer to the following steps to customize the audio sink in your project:

1. Call the `setExternalAudioSink` method to enable the external audio sink before joining a channel.
2. After you join the channel, call the `pullAudioFrame` method to get the remote audio data.
Play the remote audio data on your own.

**API call sequence**

![](https://web-cdn.agora.io/docs-files/1569384254887)

**API reference**

- [`setExternalAudioSink`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1rtc_1_1_i_rtc_engine.html#a790b4896f2769c1edebbb49d8912e38b)
- [`pullAudioFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#aaf43fc265eb4707bb59f1bf0cbe01940)

## Sample code

Agora provides an open-source [AgoraAudioIO](https://github.com/AgoraIO/Advanced-Audio/tree/master/Windows/AgoraAudioIO-Windows) demo project on GitHub. You can download it for source code reference.

## Considerations

* Ensure the accuracy and efficiency of the audio data processing in the callback methods to avoid any possible crash.
* Set the audio data to `RAW_AUDIO_FRAME_OF_MODE_READ_WRITE` if you want to read, write, and manipulate the data.
* Customizing the audio source and sink requires you to manage audio data recording and playback on your own.

	- When customizing the audio source, you need to record and process the audio data on your own.
	- When customizing the audio sink, you need to process and play back the audio data on your own.
