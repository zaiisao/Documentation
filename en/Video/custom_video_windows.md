
---
title: Customize Audio/Video Source and Renderer
description: 
platform: Windows
updatedAt: Thu Nov 15 2018 06:23:34 GMT+0000 (UTC)
---
# Customize Audio/Video Source and Renderer
## Introduction
By default, An App uses the internal audio and video modules for capturing and rendering during real-time communication. If developers hope to use external audio or video source and renderer, this page shows how to use the APIs provided by Agora SDK to customize the audio and video source and renderer.

**Customizing audio and video source and renderer** mainly applies to the following scenarios:

* When the audio or video source captured by the internal modules can not meet the needs of the developers. For example, for the purpose of image enhancement, developers need to process the captured video frame with a preprocessing library.
* When an App contains its own audio or video module and wants a cusmized source for code reuse.
* When developers want to use non-camera source, such as recorded screen data.
* When developers need flexible device resource allocation to avoid conflicts with other services.

## Implementaions
Before proceeding, ensure that you have finished preparing the development environment. See [Integrate the SDK](../../en/Video/windows_video.md) for details.

### Customize the Audio Source

```cpp
//cpp
// 1. preparations

// an audio queue for push/pop audio pcm data
CAudioPlayPackageQueue    *CAudioPlayPackageQueue::m_lpAudioPackageQueue = NULL;

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

// audio observer implementation for external audio source
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

2. enable customized audio capturing

RtcEngineParameters rep(lpAgoraEngine);

int nRet = rep.setExternalAudioSource(true, nSampleRate, nChannels);

agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
mediaEngine.queryInterface(lpAgoraEngine, agora::AGORA_IID_MEDIA_ENGINE);

mediaEngine->registerAudioFrameObserver(lpExternalAudioFrameObserver);

nRet = apm->setParameters("{\"che.audio.external_capture\":true}");

3. start push/pull audio data to/from Agora SDK
// need to maintain a thread loop

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

4. disable customized audio capturing

nRet = apm->setParameters("{\"che.audio.external_capture\":false}");

mediaEngine->registerAudioFrameObserver(NULL);
```

### Customize the Video Source

```cpp
//cpp
// 1. preparations

// a video queue for push/pop video frame data
CVideoPackageQueue *CVideoPackageQueue::m_lpVideoPackageQueue = NULL;

CVideoPackageQueue::CVideoPackageQueue()
{
    m_nPackageSize = 0;
    m_nBufferSize = 0x800000;
    m_bufQueue.Create(32, 0x800000);
}

CVideoPackageQueue::~CVideoPackageQueue()
{
    m_bufQueue.FreeAllBusyBlock();
    m_bufQueue.Close();
}

CVideoPackageQueue *CVideoPackageQueue::GetInstance()
{
    if (m_lpVideoPackageQueue == NULL)
        m_lpVideoPackageQueue = new CVideoPackageQueue();

    return m_lpVideoPackageQueue;
}

void CVideoPackageQueue::CloseInstance()
{
    if (m_lpVideoPackageQueue == NULL)
        return;

    delete m_lpVideoPackageQueue;
    m_lpVideoPackageQueue = NULL;
}

void CVideoPackageQueue::SetVideoFormat(const BITMAPINFOHEADER *lpInfoHeader)
{
  memcpy_s(&m_bmiHeader, sizeof(BITMAPINFOHEADER), lpInfoHeader, sizeof(BITMAPINFOHEADER));

  m_nPackageSize = m_bmiHeader.biWidth*m_bmiHeader.biWidth * 3 / 2;
  _ASSERT(m_nPackageSize <= m_nBufferSize);
}

void CVideoPackageQueue::GetVideoFormat(BITMAPINFOHEADER *lpInfoHeader)
{
  memcpy_s(lpInfoHeader, sizeof(BITMAPINFOHEADER), &m_bmiHeader, sizeof(BITMAPINFOHEADER));
}

BOOL CVideoPackageQueue::PushVideoPackage(LPCVOID lpVideoPackage, SIZE_T nPackageLen)
{
  if (m_bufQueue.GetFreeCount() == 0)
    m_bufQueue.FreeBusyHead(NULL, 0);

  LPVOID lpBuffer = m_bufQueue.AllocBuffer(FALSE);
  if (lpBuffer == NULL) 
    return FALSE;

  _ASSERT(m_bufQueue.GetBytesPreUnit() >= nPackageLen);

  memcpy_s(lpBuffer, m_bufQueue.GetBytesPreUnit(), lpVideoPackage, nPackageLen);

  return TRUE;
}

BOOL CVideoPackageQueue::PopVideoPackage(LPVOID lpVideoPackage, SIZE_T *nPackageSize)
{
  _ASSERT(nPackageSize != NULL);

  if (nPackageSize == 0)
    return FALSE;

  if (m_bufQueue.GetBusyCount() == 0)
    return FALSE;

  if (*nPackageSize < m_nPackageSize) {
    *nPackageSize = m_nPackageSize;
    return FALSE;
  }

  *nPackageSize = m_nPackageSize;
  m_bufQueue.FreeBusyHead(lpVideoPackage, m_nPackageSize);

  return TRUE;
}

// video observer implementation for external video source
CExternalVideoFrameObserver::CExternalVideoFrameObserver()
{
  m_lpImageBuffer = new BYTE[0x800000];
}

CExternalVideoFrameObserver::~CExternalVideoFrameObserver()
{
  delete[] m_lpImageBuffer;
}

bool CExternalVideoFrameObserver::onCaptureVideoFrame(VideoFrame& videoFrame)
{
  SIZE_T nBufferSize = 0x800000;

  BOOL bSuccess = CVideoPackageQueue::GetInstance()->PopVideoPackage(m_lpImageBuffer, &nBufferSize);
  if (!bSuccess)
    return false;

  m_lpY = m_lpImageBuffer;
  m_lpU = m_lpImageBuffer + videoFrame.height*videoFrame.width;
  m_lpV = m_lpImageBuffer + 5 * videoFrame.height*videoFrame.width / 4;

  memcpy_s(videoFrame.yBuffer, videoFrame.height*videoFrame.width, m_lpY, videoFrame.height*videoFrame.width);
  videoFrame.yStride = videoFrame.width;

  memcpy_s(videoFrame.uBuffer, videoFrame.height*videoFrame.width / 4, m_lpU, videoFrame.height*videoFrame.width / 4);
  videoFrame.uStride = videoFrame.width/2;

  memcpy_s(videoFrame.vBuffer, videoFrame.height*videoFrame.width / 4, m_lpV, videoFrame.height*videoFrame.width / 4);
  videoFrame.vStride = videoFrame.width/2;

  videoFrame.type = FRAME_TYPE_YUV420;
  videoFrame.rotation = 0;

  return true;
}

bool CExternalVideoFrameObserver::onRenderVideoFrame(unsigned int uid, VideoFrame& videoFrame)
{
  return true;
}

2. enable customized video capturing

RtcEngineParameters rep(lpAgoraEngine);

agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
mediaEngine.queryInterface(lpAgoraEngine, agora::AGORA_IID_MEDIA_ENGINE);

int mRet = apm->setParameters("{\"che.video.local.camera_index\":1024}");
nRet = mediaEngine->registerVideoFrameObserver(lpVideoFrameObserver);

3. start push/pull video data to/from Agora SDK

lpPackageQueue->PushVideoPackage(m_lpYUVBuffer, nYUVSize); // push to Agora SDK

4. disable customized video capturing

nRet = apm->setParameters("{\"che.video.local.camera_index\":0}");

mediaEngine->registerVideoFrameObserver(NULL);
```

## Considerations

* Ensure the accuracy and efficiency of audio and video data processing in the callback methods to avoid any possible crash.
* Set the audio data to `RAW_AUDIO_FRAME_OF_MODE_READ_WRITE` if you hope to read, write and manipulate the data.
* Use raw data APIs to customize the video renderer. If you do not want the SDK to render the video frame, do not call `setupLocalVideo`. Ensure compatibility on the Windows platform when customizing the video renderer.
* Customizing audio/video source and renderer is an advanced feature provided by Agora SDK. To develop this function, we believe it necessary that you have adequate knowledge and experience in audio and video application development.
