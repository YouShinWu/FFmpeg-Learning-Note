# FFmpeg Learning Note
* FFmpeg transcode
    * h264 -> h265
```
ffmpeg -i input.mp4 -c:v libx265 -vtag hvc1 -vf scale=1920:1080 -preset fast -c:a copy output.mp4
```
```-i``` input file
```-c:v``` video codec
```-c:a``` audio codec
```-vf scale=1920:1080``` specifying output resolution
```hevc_nvenc``` Using GPU for encode
```-crf 20``` Compression quality
* -crf 0 high-quality,low compression, large file
* -crf 23 default
* -crf 51 low-quality,high compression,small file

# Convert Hevc to Hls
```
ffmpeg -y -i input.mp4 -c copy -hls_segment_type mpegts -hls_time 6 -hls_list_size 10 -hls_flags delete_segments+append_list+split_by_time -hls_playlist_type vod output.m3u8
```


# FFmpeg merge video
```
ffmpeg -f concat -safe 0 -i Text.txt -c copy Test_4k_hevc.mp4
```

Text.txt
file './4k_Batman.00001.mp4'
file './4k_Batman.00002.mp4'
file './4k_Batman.00003.mp4'
file './4k_Batman.00004.mp4'
file './4k_Batman.00005.mp4'

# Show Connected Device
*Show PC connected audio and camera devices ID.
``` 
ffmpeg -list_devices true -f dshow -i dummy
```

# Push Vido to RTMP Server
* Local video
```
ffmpeg.exe -re -i input.mp4 -c copy -f flv rtmp://server_IP:RTMP_PORT/live/mmlab
```
* Webcam video only
```
ffmpeg.exe -f dshow -rtbufsize 2048M -framerate 30 -video_size 640x480 ^
    -i video="Intel(R) RealSense(TM) Depth Camera 435 with RGB Module RGB" ^
    -c:v libx264 -b:v 4000k -preset ultrafast ^
    -x264opts keyint=50 -g 25 -pix_fmt yuv420p ^
    -filter:v drawtext="fontsize=20:fontcolor='white':box=1:boxcolor='black@0.8':boxborderw=5:timecode='00\:00\:00;00':timecode_rate=(30*1000/1001):x=(w-text_w):y=(h-text_h)" ^
    -c:a copy -f flv rtmp://SERVER_IP:RTMP_PORT/live/mmlab
```
  
* Webvam video with audio
```
ffmpeg\bin\ffmpeg.exe -f dshow -framerate 25 -video_size 480x360  ^ 
    -i video="USB Video Device":audio="@device_cm_{33D9A762-90C8-11D0-BD43-00A0C911CE86}\wave_{4525AD9D-7B8B-4150-A695-58ED379919A4}" ^
    -c:v libx264 -b:v 1600k -preset ultrafast ^
    -x264opts keyint=50 -g 25 -pix_fmt yuv420p ^
    -c:a aac -b:a 128k ^
    -filter:v drawtext="fontsize=20:fontcolor='white':box=1:boxcolor='black@0.8':boxborderw=5:timecode='00\:00\:00;00':timecode_rate=(30*1000/1001):x=(w-text_w):y=(h-text_h)" ^
    -c:a copy -f flv rtmp://SERVER_IP:RTMP_PORT/live2/mmlab
```

ADD Timestamp
``` 
-filter:v drawtext="fontsize=20:fontcolor='white':box=1:boxcolor='black@0.8':boxborderw=5:timecode='00\:00\:00;00':timecode_rate=(30*1000/1001):x=(w-text_w):y=(h-text_h)"
```  

# Pull Video Stream
```
ffplay -x 720 http://SERVER_IP:HTTP_PORT/hls/mmlab.m3u8
```

```-x``` Set video size