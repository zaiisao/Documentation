
---
title: 自定义视频采集和渲染
description: 
platform: Windows
updatedAt: Wed Jul 08 2020 04:50:23 GMT+0800 (CST)
---
# 自定义视频采集和渲染
## 功能介绍

实时音视频传输过程中，Agora SDK 通常会启动默认的音视频模块进行采集和渲染。在以下场景中，你可能会发现默认的音视频模块无法满足开发需求：

- app 中已有自己的音频或视频模块
- 希望使用非 Camera 采集的视频源，如录屏数据
- 需要使用自定义的美颜库或有前处理库
- 某些视频采集设备被系统独占。为避免与其它业务产生冲突，需要灵活的设备管理策略

基于此，Agora Native SDK 支持使用自定义的音视频源或渲染器，实现相关场景。本文介绍如何实现自定义视频采集和渲染。

## 实现方法

开始自定义采集和渲染前，请确保你已在项目中实现基本的通话或者直播功能，详见[一对一通话](../../cn/Video/start_call_windows.md)或[互动直播](../../cn/Video/start_live_windows.md)。

参考如下步骤，在你的项目中实现自定义视频源功能：

1. 在 `joinChannel` 前通过调用 `setExternalVideoSource` 指定外部视频采集设备。
2. 指定外部采集设备后，开发者自行管理视频数据采集和处理。
3. 完成视频数据处理后，再通过 `pushVideoFrame` 发送给 SDK 进行后续操作。
    <div class="alert note">为满足实际使用需求，你可以在将视频数据发送回 SDK 前，通过 <code>ExternalVideoFrame</code> 修改视频数据。比如，设置 <code>rotation</code> 为 180，使视频帧顺时针旋转 180 度。</div>

### API 调用时序

参考下图时序在你的项目中自定义视频采集。

![](https://web-cdn.agora.io/docs-files/1569403362994)

### 示例代码

参考下文代码在你的项目中自定义视频采集。

```cpp
// 准备工作，需要实现视频采集模块，以及视频数据队列（用来存放采集出来的数据/或者将要渲染的数据）
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

// 实现视频观测器，为采集外部视频源做准备
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

lpAgoraEngine->enableVideo();

// 启用外部视频数据源模式，注册视频观测器，我们使用观测器将外部的数据源传递给引擎以及把引擎返回的数据给到应用
agora::util::AutoPtr<agora::media::IMediaEngine> mediaEngine;
mediaEngine.queryInterface(lpAgoraEngine, agora::AGORA_IID_MEDIA_ENGINE);

int mRet = apm->setParameters("{\"che.video.local.camera_index\":1024}"); 
nRet = mediaEngine->registerVideoFrameObserver(lpVideoFrameObserver);

// 开始往引擎推送数据以及从引擎获取数据，通常需要自己维护一个线程循环
lpPackageQueue->PushVideoPackage(m_lpYUVBuffer, nYUVSize); // push to Agora SDK

// 停止外部视频数据源模式
nRet = apm->setParameters("{\"che.video.local.camera_index\":0}");

mediaEngine->registerVideoFrameObserver(NULL);
```

同时，我们在 GitHub 提供一个开源的 [Agora-Media-Source-Windows](https://github.com/AgoraIO/Advanced-Video/tree/master/Windows/Agora-Media-Source-Windows) 示例项目。你可以下载体验或查看源代码。

### API 参考

- [`setExternalVideoSource`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#a6716908edc14317f2f6f14ee4b1c01b7)
- [`pushVideoFrame`](https://docs.agora.io/cn/Video/API%20Reference/cpp/classagora_1_1media_1_1_i_media_engine.html#ae064aedfdb6ac63a981ca77a6b315985)

## 注意事项
* 回调函数里处理音视频数据要尽量高效，且保证算法稳定，避免影响整个客户端或产生崩溃。
* 音频部分需要设置 `RAW_AUDIO_FRAME_OF_MODE_READ_WRITE` 才可以读写和操作数据。
* 自定义渲染实现，也是使用裸数据接口，不调用 `setupRemoteVideo` 就可以不使用 SDK 渲染。自定义渲染要注意 Windows 平台的兼容性。
* 自定义视频采集和渲染场景中，需要开发者具有采集或渲染视频的能力：

	- 自定义视频采集场景中，你需要自行管理视频数据的采集和处理。
	- 自定义视频渲染场景中，你需要自行管理视频数据的处理和显示。

## 相关文档

如果你还想在项目中实现自定义的音频采集和渲染功能，请参考文档[自定义音频采集和渲染](../../cn/Video/custom_audio_windows.md)。

