## gif

30 FPS、 64 色、幅 500 ピクセルの gif に変換

```
ffmpeg -y -i source.mp4 -filter_complex "[0:v] fps=30,scale=500:-1,split [a][b];[a] palettegen=max_colors=64 [p];[b][p] paletteuse=dither=floyd_steinberg" out.gif
```

## mp4

2:00 から 5:00 までオーディオ無しの mp4 に変換

```
ffmpeg -i source.mov -pix_fmt yuv420p -an -ss 2 -t 3 out.mp4
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