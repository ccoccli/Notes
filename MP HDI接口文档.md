# **1.AV模块相关接口**

> 本模块主要完成对音视频的一些操作，比如音频视播放，停止，设置音视频PID，静音，黑屏等等操作

## 1.`MP_STATUS MP_HDI_AV_Init(MP_VOID);`

> Function Description : 音视频模块初始化
>
> Input Params             : 无
>
> Output Params          : 无
>
> Return values             :   成功：MP_SUCCESS（0）  |   失败：MP_FAILURE （-1）

## 2.`MP_VOID MP_HDI_AV_Audio_SetMute(MP_BOOL bOn);`

>  Function Description :  设置音频状态为静音
>
> Input Params             :  bOn: 设置静音状态，MP_TRUE时静音（mute） |    MP_FALSE则不静音(unmute)
>
> Output Params          : 无
>
> Return values             :  无

## 3.`MP_BOOL MP_HDI_AV_Audio_GetMute(MP_VOID);`

> Function Description :  获取当前音频静音状态
>
> Input Params             : 无
>
> Output Params          : 无
>
> Return values             :  1，表示是处于静音状态     |     0，  表示未处于静音状态

## 4.`MP_UINT8 MP_HDI_AV_Audio_GetVolume(MP_VOID);`

> Function Description :  获取当前音量值,目前音量step是1，最大是31. 范围就是从（0~31）
>
> Input Params             : 无
>
> Output Params          : 无
>
> Return values             :  当前的音量值

## 5.`MP_VOID MP_HDI_AV_Audio_SetVolume(MP_UINT8 nVolume);`

> Function Description :  设置当前音量值，如果是静音状态，不允许进行设置输入参数
>
> Input Params             :  nVolume: 要设置的音量值，最大值为31，范围为（0~31）
>
> Output Params          : 无
>
> Return values             :  无

## 6.`MP_STATUS MP_HDI_AV_Audio_SetSpdifOutput(HDI_SPDIF_OUT_E format);`

> Function Description :   设置SPDIF输出格式
>
> Input Params             :   Format ( EM_HDI_SPDIF_OUT_NONE    |    EM_HDI_SPDIF_OUT_RAW                                                                      														 EM_HDI_SPDIF_OUT_PCM       |    HDI_SPDIF_OUT_NOCHANGE )
>
> Output Params          : 无
>
> Return values             :  成功：MP_SUCCESS（0）  |   失败：MP_FAILURE （-1）

## 7.`void MP_HDI_AV_Audio_SetChannelSelection(HDI_CH_SELECT_E option);`

> Function Description :  设置音频输出声道
>
> Input Params             :   option: 参考枚举HDI_CH_SELECT_E
>
> Output Params          : 无
>
> Return values             :  无

```c
typedef enum
{
    EM_HDI_CH_SELECT_STEREO,
    EM_HDI_CH_SELECT_LEFT,
    EM_HDI_CH_SELECT_RIGHT,
    EM_HDI_CH_SELECT_MONO
} HDI_CH_SELECT_E;

```

## 8.`HDI_CH_SELECT_E MP_HDI_AV_Audio_GetChannelSelection(MP_VOID);`

> Function Description :  获取音频输出声道
>
> Input Params             : 无
>
> Output Params          : 无
>
> Return values             :  当前音频输出声道，参考枚举HDI_CH_SELECT_E

## 9.`void MP_HDI_AV_Audio_Play(HDI_AUDIO_FORMAT_E aFormat,MP_UINT16 audio_pid);`

> Function Description :   播放音频, MP平台会根据PMT表的数据来播放音频, 这就导致视频和音频是分开播放的，必须在实现HDI层时，考虑到这个细节
>
> Input Params             :  1.aFormat: 音频格式，参考枚举 HDI_AUDIO_FORMAT_E
>
> ​    									2.audio_pid: 音频 PID
>
> Output Params          :  无
>
> Return values             :  无

```c
typedef enum

{
    EM_HDI_AUDIO_FORMAT_UNKNOWN     = 0,
    EM_HDI_AUDIO_FORMAT_PCM,
    EM_HDI_AUDIO_FORMAT_MPEG1,
    EM_HDI_AUDIO_FORMAT_MPEG,
    EM_HDI_AUDIO_FORMAT_MP3,
    EM_HDI_AUDIO_FORMAT_AC3,
    EM_HDI_AUDIO_FORMAT_AAC,
    EM_HDI_AUDIO_FORMAT_AAC_LATM,
    EM_HDI_AUDIO_FORMAT_MAX
} HDI_AUDIO_FORMAT_E;

```

## 10.`void MP_HDI_AV_Audio_Stop(void);`

> Function Description :   停止播放音频
>
> Input Params             :  无
>
> Output Params          :  无
>
> Return values             :  无

## 11.`void MP_HDI_AV_Video_SetBlank(MP_BOOL bBlank);`

> Function Description :    设置是否黑屏.特别需要注意的是，切记不能在调用MP_HDI_AV_Video_Stop接口时，使得屏幕黑屏，因为最终控制黑屏的是  MP_HDI_AV_Video_SetBlank。切换频道时，是黑屏还是静帧，也是通过这里的逻辑得到控制.
>
> Input Params             :  bBlank : MP_TRUE表示黑屏    |    MP_FALSE表示不黑屏
>
> Output Params          :  无
>
> Return values             :  无

## 12.`void MP_HDI_AV_Video_Zoom(MP_RECT* pZoomRect);`

> Function Description :   设置视频区域, 如果是高清平台且支持双窗口，则需要确认高清窗口输出和标清窗口输出都得以设置
>
> Input Params             :  pZoomRect: 视频矩形区域，参考结构体  MP_RECT
>
> Output Params          :  无
>
> Return values             :  无

```c
typedef struct 
{
	MP_UINT32 x;	// Top left x
	MP_UINT32 y;	// Top left y
	MP_UINT32 w;	// Width
	MP_UINT32 h;	// Hight
} MP_RECT;
```

## 13.`void MP_HDI_AV_Video_Play(HDI_VIDEO_FORMAT_E vFormat,MP_UINT16 video_pid);`

> Function Description :   启动视频播放
>
> Input Params             :   1.vFormat: 视频播放格式,参考枚举HDI_VIDEO_FORMAT_E
>
> ​     									2.video_pid: 视频PID
>
> Output Params          :  无
>
> Return values             :  无

```c
typedef enum
{
    EM_HDI_VIDEO_FORMAT_UNKNOWN		= 0,
    EM_HDI_VIDEO_FORMAT_MPEG1_E,
    EM_HDI_VIDEO_FORMAT_MPEG2_E,
    EM_HDI_VIDEO_FORMAT_MPEG4_E,
    EM_HDI_VIDEO_FORMAT_H264,
	EM_HDI_VIDEO_FORMAT_H265,
    EM_HDI_VIDEO_FORMAT_AVS,
    EM_HDI_VIDEO_FORMAT_MAX
} HDI_VIDEO_FORMAT_E;
```

## 14.`void MP_HDI_AV_Video_Stop(void);`

> Function Description :   停止视频播放, 切记停止时，保留最后一帧视频图像，不能够关闭最后一帧的显示
>
> Input Params             :   无
>
> Output Params          :  无
>
> Return values             :  无

## 15.`MP_STATUS MP_HDI_AV_Video_SetPcrPid(MP_UINT16 pcr_pid);`

> Function Description :   设置播放视频的PCR PID, 在实际的一次播放过程中，这个接口只会被设置一次,而且只会在要播放video和audio时，去设置一次参数，不会进行重复设置.当PID无效时，则需返回失败
>
> Input Params             :    pcr_pid: PCR PID值
>
> Output Params          :  无
>
> Return values             :  成功：MP_SUCCESS（0）  |   失败：MP_FAILURE （-1）

## 16.`MP_STATUS MP_HDI_AV_RegisterEventNotification(HDI_AV_EVENT_TYPE_E eventType,HDI_AV_EVENT_NOTIFY callback);`

> Function Description :    注册AV事件回调
>
> Input Params             :     1.eventType: AV事件类型，参考HDI_AV_EVENT_TYPE_E
>
> ​										   2.callback: 对应AV事件类型的相关回调函数，参考原型函数HDI_AV_EVENT_NOTIFY
>
> Output Params          :  无
>
> Return values             :  成功：MP_SUCCESS（0）  |   失败：MP_FAILURE （-1）

```c
typedef enum _HDI_AV_EVENT_TYPE
{
	EM_HDI_AV_EVENT_DISPLAY_ON,		// first frame is ready
	EM_HDI_AV_FRAME_PICTURE_READY,	//one frame is decoded ready
	EM_HDI_AV_EVENT_MAX
}HDI_AV_EVENT_TYPE_E;
```

