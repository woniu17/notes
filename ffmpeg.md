# 音频
1. CD未经压缩的音频
    1. 采样数据: 16 bit
    1. 采样频率: 44100 HZ
    1. 通道: 2
    1. 比特率（bitrate）: 16 bit/采样点 * 44100 采样点/秒 * 2 通道 = 1411.2 kbps
1. CD上，每一秒钟的音频的容量 176 KB
1. CD上，一段一分钟的音频的容量 10 MB左右（10560 KB）

## ffprobe
    ```
    Input #0, mp3, from '01.mp3':
      Metadata:
        major_brand     : mp42
        minor_version   : 0
        compatible_brands: isommp42
        encoder         : Lavf57.56.101
      Duration: 00:51:26.65, start: 0.025057, bitrate: 107 kb/s
        Stream #0:0: Audio: mp3, 44100 Hz, stereo, s16p, 107 kb/s
        Metadata:
          encoder         : Lavc57.64
    ```

    > Input #0, mp3, from '01.mp3':
    1. : Input #0
    1. : mp3

    > Audio: mp3, 44100 Hz, stereo, s16p, 107 kb/s
    1. codec name : mp3
    1. sample rate / 采样频率 : 44100 Hz
    1. channel layout : stereo / 双声道 ; mono / 单声道
    1. sample format : s16p / signed 16 bits, planar ; fltp / float, planar
    1. bit rate : 107 kb/s

    ```
    Input #0, mov,mp4,m4a,3gp,3g2,mj2, from '01.mp4':
      Metadata:
        major_brand     : mp42
        minor_version   : 0
        compatible_brands: isommp42
        creation_time   : 2016-11-28T04:55:51.000000Z
      Duration: 00:51:26.63, start: 0.000000, bitrate: 556 kb/s
        Stream #0:0(und): Video: h264 (Constrained Baseline) (avc1 / 0x31637661), yuv420p, 540x360 [SAR 1:1 DAR 3:2], 457 kb/s, 29.97 fps, 29.97 tbr, 30k tbn, 59.94 tbc (default)
        Metadata:
          handler_name    : VideoHandler
        Stream #0:1(und): Audio: aac (LC) (mp4a / 0x6134706D), 44100 Hz, stereo, fltp, 95 kb/s (default)
        Metadata:
          creation_time   : 2016-11-28T04:56:17.000000Z
          handler_name    : IsoMedia File Produced by Google, 5-11-2011
    ```
    > Input #0, mov,mp4,m4a,3gp,3g2,mj2, from '01.mp4':
    1. : Input #0
    1. : mov,mp4,m4a,3gp,3g2,mj2

