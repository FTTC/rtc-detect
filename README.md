## 探测环境插件
rtc-detect 用来检测当前环境对 TRTC SDK 的支持度。

## 安装
```shell
npm install rtc-detect
```

## 使用方法
```javascript
import RTCDetect from 'rtc-detect';
// 初始化监测模块
const detect = new RTCDetect();
// 获得当前环境监测结果
const result = await detect.getReportAsync();
console.log('result is: ' + result);
```

## API
### (async) isTRTCSupported()
判断当前环境是否支持 TRTC。
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
获取当前系统环境参数。

| Item              | Type   |      Description             |
| ----------------- | ------ | ---------------------------- |
| UA                | string | 浏览器的 ua                   |
| OS                | string | 当前设备的系统型号              |
| browser           | object | 当前浏览器信息{ name, version } |
| displayResolution | object | 当前分辨率 { width, height }   |
| getHardwareConcurrency | number | 当前设备 CPU 核心数 |


```javascript
const detect = new RTCDetect();
const result = detect.getSystem();
```

### getAPISupported()
获取当前环境 API 支持度。

| Item                              | Type    |      Description       |
| --------------------------------- | ------- | ---------------------- |
| isUserMediaSupported              | boolean | 是否支持获取用户媒体数据流               |
| isWebRTCSupported                 | boolean | 是否支持 WebRTC               |
| isWebSocketSupported              | boolean | 是否支持 WebSocket       |
| isWebAudioSupported               | boolean | 是否支持 WebAudio       |
| isScreenCaptureAPISupported       | boolean | 是否支持获取屏幕的流       |
| isCanvasCapturingSupported        | boolean | 是否支持从 canvas 获取数据流     |
| isVideoCapturingSupported         | boolean | 是否支持从 video 获取数据流       |
| isRTPSenderReplaceTracksSupported | boolean | 是否支持替换 track 时不和 peerConnection 重新协商    |
| isApplyConstraintsSupported       | boolean | 是否支持变更摄像头的分辨率不通过重新调用 getUserMedia       |


```javascript
const detect = new RTCDetect();
const result = detect.getAPISupported();
```


### (async) getDevicesAsync()
获取当前环境可用的设备。

| Item                              | Type    |      Description       |
| --------------------------------- | ------- | ---------------------- |
| hasWebCamPermissions              | boolean | 是否支持获取用户摄像头数据   |
| hasMicrophonePermission           | boolean | 是否支持获取用户麦克风数据   |
| cameras                           | array   | 用户的摄像头设备列表，包含支持视频流的最大宽高和帧率 |
| microphones                       | array   | 用户的麦克风设备列表        |
| speakers                          | array   | 用户的扬声器设备列表        |


```javascript
const detect = new RTCDetect();
const result = await detect.getDevicesAsync();
```



### (async) getCodecAsync()
获取当前环境参数对编码的支持度。

| Item                           | Type    |      Description    |
| ------------------------------ | ------- | ------------------- |
| isH264EncodeSupported          | boolean | 是否支持 h264 上行       |
| isH264DecodeSupported          | boolean | 是否支持 h264 下行       |
| isVp8EncodeSupported           | boolean | 是否支持 vp8 上行       |
| isVp8DecodeSupported           | boolean | 是否支持 vp8 下行       |


```javascript
const detect = new RTCDetect();
const result = await detect.getCodecAsync();
```


### (async) getReportAsync()
获取当前环境监测报告。

| Item                 | Type    |      Description                 |
| -------------------- | ------- | -------------------------------- |
| system               | object  | 和 getSystem() 的返回值一致        |
| APISupported         | object  | 和 getAPISupported() 的返回值一致  |
| codecsSupported      | object  | 和 getCodecAsync() 的返回值一致    |
| devices              | object  | 和 getDevicesAsync() 的返回值一致  |


```javascript
const detect = new RTCDetect();
const result = await detect.getReportAsync();
```

### (async) isHardWareAccelerationEnabled()

检测 Chrome 浏览器是否开启硬件加速。

注意：该接口的实现依赖于 WebRTC 原生接口，建议在 isTRTCSupported 检测支持后，再调用该接口进行检测。检测最长耗时 30s。经实测：
1. 开启硬件加速的情况下，该接口在 Windows 耗时 2s 左右， Mac 需耗时 10s 左右。
2. 关闭硬件加速的情况下，该接口在 Windows 和 Mac 耗时均为 30s。

```javascript
const detect = new RTCDetect();
const data = await detect.isTRTCSupported();

if (data.result) {
  const result = await detect.isHardWareAccelerationEnabled();
  console.log(`is hardware acceleration enabled: ${result}`);
} else {
  console.log(`current browser does not support TRTC, reason: ${data.reason}.`)
}
```


