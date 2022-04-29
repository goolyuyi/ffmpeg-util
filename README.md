## Syntax

```
ffmpeg [global_options] {[input_file_options] -i input_url} ... {[output_file_options] output_url} ...
```

## Help

* `ffmpeg --help muxer=mp4`
* `ffmpeg --help encoder=mov_text`
* `ffmpeg -formats`
* https://trac.ffmpeg.org/wiki
* https://cheatography.com/thetartankilt/cheat-sheets/ffmpeg/
* https://developer.mozilla.org/en-US/docs/Web/Media/Formats

## Stream specifiers

* `a:1` refers to the second audio stream in the first input file
* `0:a:1` refers to the second audio stream in the first input file
* `v` for video
* `V` only matches video streams which are not attached pictures, video thumbnails or cover arts
* `a` for audio
* `s` for subtitle
* `d` for data
* `t` for attachments.

## Map

* `ffmpeg -i INPUT -map 0 output`
* `ffmpeg -i INPUT -map 0:v -map 0:a:2 OUTPUT`
* `ffmpeg -i a.mov -i b.mov -c copy -map 0:2 -map 1:6 out.mov`
* `ffmpeg -i INPUT -map 0:v -map 0:a:2 OUTPUT` a,v must be separated
* `ffmpeg -i INPUT -map 0 -map -0:a:1 OUTPUT` exclude map
* `ffmpeg -i INPUT -map 0:v -map 0:a? OUTPUT` optional, won`t fail if there is no audio
* `-an -vn -sn` disable audio/video/subtitle

## Metadata

* `ffmpeg -i in.avi -metadata title="my title" out.flv` set title
* `ffmpeg -i INPUT -metadata:s:a:0 language=eng OUTPUT` set language of the first audio stream
* `ffmpeg -i in.mkv -c copy -disposition:a:1 default out.mkv` reorder streams
* `ffmpeg -i in.mkv -c copy -disposition:s:0 0 -disposition:s:1 default out.mkv` to make the second subtitle stream the
  default stream and remove the default disposition from the first subtitle stream

## Filters:

* drawtext: https://ffmpeg.org/ffmpeg-filters.html#drawtext-1
* transpose: https://ffmpeg.org/ffmpeg-filters.html#toc-transpose-1
* fps:https://ffmpeg.org/ffmpeg-filters.html#toc-fps-1
* vflip: https://ffmpeg.org/ffmpeg-filters.html#vflip
* hflip
*
*

## Codec:

* common option: https://ffmpeg.org/ffmpeg-codecs.html#Codec-Options
  `-b 1M -ab 128k -ar 44100 -ac 2`

### H264

* https://trac.ffmpeg.org/wiki/Encode/H.264
* `ffmpeg -i i.mp4 -tune animation -y o.mp4`
* `ffmpeg -i input -c:v libx264 -preset slow -crf 22 -c:a copy output.mkv`
* -crf 0~51, default 23, lower is better. 17–28 is a good range.
* two pass

```
ffmpeg -y -i input -c:v libx264 -b:v 2600k -pass 1 -an -f null /dev/null && \
ffmpeg -i input -c:v libx264 -b:v 2600k -pass 2 -c:a aac -b:a 128k output.mp4
 ```

* excellent quality: `-preset slower -crf 18`
* https://slhck.info/video/2017/03/01/rate-control.html

## Container/Format:

* common option: https://ffmpeg.org/ffmpeg-formats.html#Format-Options
* `-fflag fastseek`

## ffprobe

* `ffprobe -show_format -of json input.mkv`
* `ffprobe -show_streams -select_streams v -of json input.mkv`

## Examples

* `ffmpeg -i foo.avi -r 1 -s WxH -f image2 foo-%03d.jpeg`
  This will extract one video frame per second from the video and will output them in files named foo-001.jpeg,
  foo-002.jpeg, etc. Images will be rescaled to fit the new WxH values.

* ```
  ffmpeg -i subtitle.vtt o.ass
  ffmpeg -i video.webm -vf subtitles=o.ass out.mp4
  ```

  bake subtitles into video(for youtube dump)
  https://trac.ffmpeg.org/wiki/HowToBurnSubtitlesIntoVideo
* ```
  ffmpeg -vf "drawtext=text='for animator %{frame_num}': start_number=1: x=(w-tw)/2: y=h-(2*lh): fontcolor=black: fontsize=30: box=1: boxcolor=white: boxborderw=5" -c:a copy -i xxx.mp4 output.mp4
  ```

* Change FPS: https://trac.ffmpeg.org/wiki/ChangingFrameRate

## TODO

* `ffmpeg -i input.mkv -c copy -c:s mov_text output.mp4`
* dvdsub
* Mp4 vs mkv
* `ffmpeg -i input.mkv -vf subtitles=input.mkv output.mp4`
* `-c:s dvd_subtitle`
* audio:

    * ac3
    * aac

## Development

* http://dranger.com/ffmpeg/tutorial01.html
* https://www.libsdl.org/

