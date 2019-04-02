---
title:  H5 直播系统开发之 ffmpeg 命令和 fluent-ffmpeg 实现视频推流
key: sum-2019-0103
tags:  直播
comment: true
# modify_date:  +08:00
# excerpt_type: html 
---

## 1. 使用 ffmpeg 命令推流

### 准备视频流地址或者视频文件都可以

比如我使用的是海康摄像头，地址为：
```sh
rtsp://admin:123456@192.168.9.33:554/Streaming/Channels/101
```

### ffmpeg 推流命令

当然前提是你已经搭建好了 视频流服务器。

```sh
ffmpeg -i rtsp://admin:swjtu9422@192.168.9.33:554/Streaming/Channels/101 -f mp4 -vcodec copy -r 25 -s 1920*1080 -b:v 1024000 -an -f flv -an rtmp://192.168.9.12:1935/live/stream
```

## 2. Nodejs 使用 fluent-ffmpeg 推流

nodejs 安装 fluent-ffmpeg 模块，使用 fluent-ffmpeg 模块执行 ffmpeg 命令完成推流

```js
// 安装
npm i fluent-ffmpeg

// 使用

var inputPath = 'rtsp://admin:swjtu9422@192.168.9.34:554/Streaming/Channels/101'
var outPath = 'rtmp://192.168.9.12:1935/live/stream'

ffmpeg(inputPath)
  // .inputOptions('-re')
  .on('start', function (commandLine) {
      console.log('ffmpeg 命令: ', commandLine)
  })
  .on('error',function (err,stdout, stderr) {
      console.log('error: ' + err.message)
      console.log('stdout: ' + stdout)
      console.log('stderr: ' + stderr)
  })
  // .on('progress', function (progress) {
  //     console.log('progressing: ', progress.percent , ' % done')
  // })
  .on('stderr', function(stderrLine) {
  console.log('output: ' + stderrLine);
  })
  .on('end', function () {
      console.log('完成 ')
  })
  .addOptions([
      // '-vcodec h264',
      '-f mp4',
      '-an',
      '-vcodec copy',
      '-r 25',
      '-s 1920*1080',
      '-b:v 1024000'
      // '-vcodec libx264',
      // '-c:a aac',
      // '-bufsize 3000k',
      // '-max_muxing_queue_size 1024',
      // '-preset veryfast', // 牺牲视频质量，换取流畅性
      // '-ac 2', // 双声道输出
      // '-ar 44100' // 音频采样率
  ])
  // .noAudio()
  // .videoCodec('copy')
  .format('flv')
  // .format('h264')
  // .pipe(outPath, {end: true})
  .output(outPath)  // 使用 pipe 管道 ，output 和 run 不可用
  .run()
```

### 1. 原视频流 rtsp 播放测试
![](http://img.fuwenwei.com/blog/rtsp播放2.png)

### 2. rtmp 播放
![](http://img.fuwenwei.com/blog/rtmp播放.png)

### 3. hls 播放
![](http://img.fuwenwei.com/blog/m3u8播放.png)

### 4. http-flv 播放
![](http://img.fuwenwei.com/blog/http-flv播放.png)

### 5. http-flv和rtmp和rtsp播放延时对比
![](http://img.fuwenwei.com/blog/http-flv和rtmp和rtsp播放延时对比.png)

### 6. hls和rtmp和rtsp播放延时对比
![](http://img.fuwenwei.com/blog/m3u8和rtmp和rtsp播放延时对比.png)

### 7. http-flv和m3u8播放延时对比
![](http://img.fuwenwei.com/blog/http-flv和m3u8播放延时对比.png)


## 播放地址

```
rtsp://admin:123456@192.168.9.33:554/Streaming/Channels/101

rtmp://192.168.9.12:1935/live/stream
http://192.168.9.12:8080/live?app=live&stream=stream

rtmp://192.168.9.12:1935/hls/stream
http://192.168.9.12:8080/hls/stream.m3u8
```
