# Unity WebGL Player

Random notes about Unity Web (WebGL Player)

## WebGL builds

### Hosting on GitHub Pages

- It needs to enable the "Decompression Fallback" switch in the Project Settings to support hosting on GitHub Pages.

### Memo

- The build size can be drastically reduced by changing the Code Optimization option to “Disk Size with LTO,” but this requires a very long build time.

## WebGPU

https://discussions.unity.com/t/early-access-to-the-new-webgpu-backend-in-unity-2023-3/933493

It requires manually modifing `ProjectSettings.asset` to enable the feature.

### Compatibility

- It runs on Chrome/Edge. It also runs on Chrome on mobiles.
- It doesn't run on Safari/Firefox. There is no way to run it on iPhone.

### Memo

- Initially, it didn’t support VFX Graph, but support was added at some point.
- It doesn't support HDRP at the moment.
- It has a color banding issue, which is especially noticeable with bloom effects.
  - It seems to be a gamma/linear conversion issue.
