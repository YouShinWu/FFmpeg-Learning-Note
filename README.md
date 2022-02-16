# FFmpeg Learning Note
* FFmpeg transcode
    * h264 -> h265
```
ffmpeg -i input.mp4 -c:v libx265 -vtag hvc1 -vf scale=1920:1080 -preset fast -c:a copy output.mp4
```
-i input file
-c:v video codec
-c:a audio codec
-vf scale=1920:1080 specifying output resolution
hevc_nvenc Using GPU for encode
-crf 20 Compression quality
 * -crf 0 high-quality,low compression, large file
 * -crf 23 default
 * -crf 51 low-quality,high compression,small file