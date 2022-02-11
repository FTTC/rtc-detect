<p align="center">
  <a href="https://intl.cloud.tencent.com/products/trtc">
    <img width="200" src="https://web.sdk.qcloud.com/trtc/webrtc/assets/trtc-logo.png">
  </a>
</p>

<h1 align="center">rtc-detect</h1>

<div align="center">

rtc-detect is used to detect whether the current environment is working smoothly in WebRTC application built by TRTC SDK.

[![NPM version](https://img.shields.io/npm/v/rtc-detect)](https://www.npmjs.com/package/rtc-detect) [![NPM downloads](https://img.shields.io/npm/dw/rtc-detect)](https://www.npmjs.com/package/rtc-detect) [![trtc.js](https://img.shields.io/bundlephobia/min/rtc-detect)](https://www.npmjs.com/package/rtc-detect) [![Documents](https://img.shields.io/badge/-Documents-blue)](https://github.com/FTTC/rtc-detect) [![Stars](https://img.shields.io/github/stars/FTTC/rtc-detect?style=social)](https://github.com/FTTC/rtc-detect)

</div>

English | [简体中文](https://github.com/FTTC/rtc-detect/blob/master/README.zh-cn.md)

## Introduction
rtc-detect is used to detect whether the current environment is working smoothly in WebRTC application built by TRTC SDK.

## Install
```shell
npm install rtc-detect
```

## How to use
```javascript
import RTCDetect from 'rtc-detect';
// init
const detect = new RTCDetect();
// get detect result
const result = await detect.getReportAsync();
console.log('result is: ' + result);
```

## API
### (async) isTRTCSupported()
Determines if the current environment supports TRTC.

```javascript
const detect = new RTCDetect();
const data = await detect.isTRTCSupported();

if (data.result) {
  console.log('current browser supports TRTC.')
} else {
  console.log(`current browser does not support TRTC, reason: ${data.reason}.`)
}
```


### getSystem()
Get the current system environment parameters.

| Item              | Type   | Description                           |
| ----------------- | ------ |---------------------------------------|
| UA                | string | user agent                            |
| OS                | string | system                                |
| browser           | object | browser infomation: { name, version } |
| displayResolution | object | resulution: { width, height }         |
| getHardwareConcurrency | number | current device CPU core count         |


```javascript
const detect = new RTCDetect();
const result = detect.getSystem();
```

### getAPISupported()
Get the current environment API support.

| Item                              | Type    | Description                                                                                          |
| --------------------------------- | ------- |------------------------------------------------------------------------------------------------------|
| isUserMediaSupported              | boolean | whether to support getting user media from media device                                                  |
| isWebRTCSupported                 | boolean | whether to support WebRTC                                                                            |
| isWebSocketSupported              | boolean | whether to support WebSocket                                                                         |
| isWebAudioSupported               | boolean | whether to support WebAudio                                                                          |
| isScreenCaptureAPISupported       | boolean | whether to support getting media steam from screen                                                   |
| isCanvasCapturingSupported        | boolean | whether to support getting media stream from canvas element                                              |
| isVideoCapturingSupported         | boolean | whether to support getting media stream from video element                                               |
| isRTPSenderReplaceTracksSupported | boolean | whether to support not renegotiating with peerConnection when replacing track                        |
| isApplyConstraintsSupported       | boolean | whether to support changing the resolution of the camera without re-calling getUserMedia |


```javascript
const detect = new RTCDetect();
const result = detect.getAPISupported();
```


### (async) getDevicesAsync()
Get the devices available for the current environment.

| Item                              | Type    | Description                                                                           |
| --------------------------------- | ------- |---------------------------------------------------------------------------------------|
| hasCameraPermission               | boolean | whether to have camera permission                                                     |
| hasMicrophonePermission           | boolean | whether to have microphone permission                                                 |
| cameras                           | array   | User's camera device list with maximum aspect and frame rate of supported video streams |
| microphones                       | array   | microphone list                                                                       |
| speakers                          | array   | speaker list                                                                          |


```javascript
const detect = new RTCDetect();
const result = await detect.getDevicesAsync();
```



### (async) getCodecAsync()
Get the support of the current environment parameters for encoding and decoding.

| Item                           | Type    | Description                      |
| ------------------------------ | ------- |----------------------------------|
| isH264EncodeSupported          | boolean | whether to support h264 uplink   |
| isH264DecodeSupported          | boolean | whether to support h264 downlink |
| isVp8EncodeSupported           | boolean | whether to support vp8 uplink    |
| isVp8DecodeSupported           | boolean | whether to support vp8 downlink    |


```javascript
const detect = new RTCDetect();
const result = await detect.getCodecAsync();
```

### (async) getReportAsync()
Get current environmental monitoring reports.

| Item                 | Type    | Description                |
| -------------------- | ------- |----------------------------|
| system               | object  | same as getSystem() result |
| APISupported         | object  | same as getAPISupported() result |
| codecsSupported      | object  | same as getCodecAsync() result   |
| devices              | object  | same as getDevicesAsync() result |


```javascript
const detect = new RTCDetect();
const result = await detect.getReportAsync();
```

### (async) isHardWareAccelerationEnabled()

Detects if Chrome is hardware acceleration enabled.

Note: The implementation of this interface depends on the WebRTC native interface, so it is recommended to call this interface after isTRTCSupported detection support. The maximum detection time is 30 s. As tested: 
1. with hardware acceleration on, the interface takes about 2s for Windows and 10s for Mac. 
2. With hardware acceleration off, the interface takes 30s on both Windows and Mac.

Translated with www.DeepL.com/Translator (free version)

```javascript
const detect = new RTCDetect();
const data = await detect.isTRTCSupported();

if (data.result) {
  const result = await detect.isHardWareAccelerationEnabled();
  console.log(`is hardware acceleration enabled: ${result}`);
} else {
  console.log(`hardware acceleration is disabled`)
}
```

## Changelog

### Version 0.0.5 @2022.02.11

**Feature**

- The `camera` object obtained from the `getDevicesAsync()` method has a new `maxFrameRate` parameter indicating the maximum frame rate supported by the camera.

### Version 0.0.4 @2021.09.06

**Improvement**

- Add reasons for detecting when WebRTC is not supported.

### Version 0.0.3 @2021.08.09

**Feature**

- Added `isHardWareAccelerationEnable()` method to detect if Chrome has hardware acceleration enabled.

### Version 0.0.2 @2021.07.24

**Improvement**

- Optimize the naming of some parameters.

### Version 0.0.1 @2021.07.13

- publish rtc-detect@0.0.1.





