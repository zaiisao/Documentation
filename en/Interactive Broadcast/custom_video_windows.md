
---
title: Custom Video Source and Renderer
description: 
platform: Windows
updatedAt: Wed May 27 2020 02:44:47 GMT+0800 (CST)
---
# Custom Video Source and Renderer
## Introduction

By default, the Agora SDK uses default audio and video modules for capturing and rendering in real-time communications. 

However, the default modules might not meet your development requirements, such as in the following scenarios:

- Your app has its own audio or video module.
- You want to use a non-camera source, such as recorded screen data.
- You need to process the captured video with a pre-processing library for functions such as image enhancement.
- You need flexible device resource allocation to avoid conflicts with other services.

This article tells you how to use the Agora Native SDK to customize the video source and renderer.

## Implementation

Before customizing the video source or renderer, ensure that you have implemented the basic real-time communication functions in your project. For details, see [Start a Call](../../en/Interactive%20Broadcast/start_call_windows.md) or [Start a Live Broadcast](../../en/Interactive%20Broadcast/start_live_windows.md).

Refer to the following steps to customize the video source:

1. Call the `setExternalVideoSource` method before `joinChannel` to enable the external video source.
2. Manage video data capturing and processing on your own.
3. Send the video data back to the SDK using the `pushVideoFrame` method.

### API call sequence

Refer to the following diagram to customize the video source in your project.

![](https://web-cdn.agora.io/docs-files/1569402488491)

### Sample code

Refer to the following code to customize the video source in your project.

```cpp
// Preparation. Implement the video capture and the video playback queue to store the captured data or data to be rendered.
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

// Implement the video observer for the external video source.
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

// Enable the external video source mode and register the video observer. Use the observer to pass the video data from the external source to the engine and then from the engine to the app.
agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
mediaEngine.queryInterface(lpAgoraEngine, agora::AGORA_IID_MEDIA_ENGINE);

int mRet = apm->setParameters("{\"che.video.local.camera_index\":1024}"); 
nRet = mediaEngine->registerVideoFrameObserver(lpVideoFrameObserver);

// Start pushing the data into the engine and retrieve the data from the engine. You may need to maintain a process loop.
lpPackageQueue->PushVideoPackage(m_lpYUVBuffer, nYUVSize); // Push to the Agora SDK.

// Disable the external video source mode.
nRet = apm->setParameters("{\"che.video.local.camera_index\":0}");

mediaEngine->registerVideoFrameObserver(NULL);
```

We provide an open source [Agora-Media-Source-Windows](https://github.com/AgoraIO/Advanced-Video/tree/master/Windows/Agora-Media-Source-Windows) demo project on GitHub. You can try the demo or view the source code.

### API reference

- [`setExternalVideoSource`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)
- [`pullVideoFrame`](https://docs.agora.io/en/Interactive%20Broadcast/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)

## Considerations

* Ensure the accuracy and efficiency of the audio and video data processing in the callback methods to avoid any possible crash.
* Set the audio data to `RAW_AUDIO_FRAME_OF_MODE_READ_WRITE` if you want to read, write, and manipulate the data.
* Use raw data methods to customize the video renderer. If you do not want the SDK to render the video frame, do not call the `setupLocalVideo` method. Ensure compatibility on the Windows platform when customizing the video renderer.
* Customizing the video source and sink requires you to manage video data recording and playback on your own.

	- When customizing the video source, you need to capture and process the video data on your own.
	- When customizing the video sink, you need to process and render the video data on your own.

## Reference

Refer to [Custom Audio Source and Sink](../../en/Interactive%20Broadcast/custom_audio_windows.md) if you want to customize the audio source and sink in your project.

