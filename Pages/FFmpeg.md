## 時間指定

2:00 から 5:00 までを切り出す

```
ffmpeg -i source.mov -ss 2 -t 3 out.mp4
```

## H.264

- `pix_fmt` で明示的に `yuv420p` を指定した方が互換性は高くなる。
- 画質の調整には `crf` が便利。10 以下でかなりの高画質になり、0 で lossless が使用できる。

```
ffmpeg -i source.mov -pix_fmt yuv420p -crf 5 out.mp4
```

## gif

30 FPS、 64 色、幅 500 ピクセルの gif に変換

```
ffmpeg -y -i source.mp4 -filter_complex "[0:v] fps=30,scale=500:-1,split [a][b];[a] palettegen=max_colors=64 [p];[b][p] paletteuse=dither=floyd_steinberg" out.gif
```

## Test video

libavfilter によるテストビデオの生成

```
ffmpeg -f lavfi -i testsrc=duration=10:size=1920x1080:rate=30 out.mp4
```

## X11

FHD, 30 fps で画面全体をキャプチャー

```
ffmpeg -f x11grab -framerate 30 -video_size 1920x1080 -i :1+0,0 -c:v huffyuv -an desktop.avi
```
