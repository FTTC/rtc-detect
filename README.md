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
This API is used to check whether the current environment supports TRTC.

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

This API is used to get the current system environment parameters.

| Item                   | Type     | Description                           |
|------------------------|----------|---------------------------------------|
| UA                     | string   | user agent                            |
| OS                     | string   | system                                |
| browser                | object   | browser infomation: { name, version } |
| displayResolution      | object   | resulution: { width, height }         |
| getHardwareConcurrency | number   | current device CPU core count         |


```javascript
const detect = new RTCDetect();
const result = detect.getSystem();
```

### getAPISupported()

This API is used to get the API support of the current environment.

| Item                               | Type      | Description                                                                              |
|------------------------------------|-----------|------------------------------------------------------------------------------------------|
| isUserMediaSupported               | boolean   | whether to support getting user media from media device                                  |
| isWebRTCSupported                  | boolean   | whether to support WebRTC                                                                |
| isWebSocketSupported               | boolean   | whether to support WebSocket                                                             |
| isWebAudioSupported                | boolean   | whether to support WebAudio                                                              |
| isScreenCaptureAPISupported        | boolean   | whether to support getting media steam from screen                                       |
| isCanvasCapturingSupported         | boolean   | whether to support getting media stream from canvas element                              |
| isVideoCapturingSupported          | boolean   | whether to support getting media stream from video element                               |
| isRTPSenderReplaceTracksSupported  | boolean   | whether to support not renegotiating with peerConnection when replacing track            |
| isApplyConstraintsSupported        | boolean   | whether to support changing the resolution of the camera without re-calling getUserMedia |


```javascript
const detect = new RTCDetect();
const result = detect.getAPISupported();
```


### (async) getDevicesAsync()

This API is used to get the available devices in the current environment.

| Item                     | Type               | Description                                                                                                                                                                                           |
|--------------------------|--------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| hasWebCamPermissions     | boolean            | Whether the user camera data can be obtained                                                                                                                                                          |
| hasMicrophonePermission  | boolean            | Whether the user mic data can be obtained                                                                                                                                                             |
| cameras                  | array<CameraItem>  | A list of the user's camera devices, including information on the resolution of supported video streams, maximum aspect and maximum frame rate (maximum frame rate is not supported by some browsers) |
| microphones              | array<DeviceItem>  | A list of user mics                                                                                                                                                                                   |
| speakers                 | array<DeviceItem>  | A list of user speakers                                                                                                                                                                               |

**CameraItem**

| Item       | Type    | Description                                                                                                                                                    |
|------------|---------|----------------------------------------------------------------------------------------------------------------------------------------------------------------|
| deviceId   | string  | Device ID, which is usually unique and can be used to capture identifying devices                                                                              |
| groupId    | string  | Group identifier, two devices have the same group identifier if they belong to the same physical device                                                        |
| kind       | string  | Camera device type: 'videoinput'                                                                                                                               |
| label      | string  | Label describing this device                                                                                                                                   |
| resolution | object  | Information about the camera's supported resolutions, maximum width and height, and maximum frame rate, eg: {maxWidth: 1280, maxHeight: 720, maxFrameRate: 30} |

**DeviceItem**

| Item     | Type     | Description                                                                                             |
|----------|----------|---------------------------------------------------------------------------------------------------------|
| deviceId | string   | Device ID, which is usually unique and can be used to capture identifying devices                       |
| groupId  | string   | Group identifier, two devices have the same group identifier if they belong to the same physical device |
| kind     | string   | Physical device type, eg: 'audioinput', 'audiooutput'                                                   |
| label    | string   | Label describing this device                                                                            |

```javascript
const detect = new RTCDetect();
const result = await detect.getDevicesAsync();
```



### (async) getCodecAsync()

This API is used to get the codec support of the current environment.

| Item                   | Type      | Description                        |
|------------------------|-----------|------------------------------------|
| isH264EncodeSupported  | boolean   | whether to support h264 uplink     |
| isH264DecodeSupported  | boolean   | whether to support h264 downlink   |
| isVp8EncodeSupported   | boolean   | whether to support vp8 uplink      |
| isVp8DecodeSupported   | boolean   | whether to support vp8 downlink    |


```javascript
const detect = new RTCDetect();
const result = await detect.getCodecAsync();
```

### (async) getReportAsync()

This API is used to get the detection report of the current environment.

| Item             | Type      | Description                        |
|------------------|-----------|------------------------------------|
| system           | object    | same as getSystem() result         |
| APISupported     | object    | same as getAPISupported() result   |
| codecsSupported  | object    | same as getCodecAsync() result     |
| devices          | object    | same as getDevicesAsync() result   |


```javascript
const detect = new RTCDetect();
const result = await detect.getReportAsync();
```

### (async) isHardWareAccelerationEnabled()

This API is used to check whether hardware acceleration is enabled on the Chrome browser.

Note: the implementation of this API depends on the native WebRTC API. We recommend you call this API for check after calling `isTRTCSupported`. The check can take up to 30 seconds as tested below:

1. If hardware acceleration is enabled, this API will take about 2 seconds on Windows and 10 seconds on macOS.
2. If hardware acceleration is disabled, this API will take about 30 seconds on both Windows and macOS.

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

### Version 1.0.0 @2023.03.17

**Bug Fixed**

- Fixed resource usage on Safari.

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





