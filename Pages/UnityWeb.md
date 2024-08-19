# Unity Web Player

Random notes about Unity Web (WebGL/WebGPU)

## Web Builds

### Hosting on GitHub Pages

- It needs to enable the "Decompression Fallback" switch in the Project Settings
  to support hosting on GitHub Pages.

### Build Size Optimization

- The build size can be drastically reduced by changing the Code Optimization
  option to “Disk Size with LTO,” but this requires a very long build time.

## WebGPU

https://discussions.unity.com/t/early-access-to-the-new-webgpu-backend-in-unity-2023-3/933493

It requires manually modifing `ProjectSettings.asset` to enable the feature.

### Compatibility

- It runs on Chrome/Edge. It also runs on Chrome on mobiles.
- It doesn't run on Safari/Firefox. There is no way to run it on iPhone.

### Memo

- Initially, it didn’t support VFX Graph, but support was added at some point.
- It doesn't support HDRP at the moment. [[HDRP]]
- It has a color banding issue, which is especially noticeable with bloom effects.
  - It has been already reported and fixed in the dev branch.
- WebGPU is only enabled in secure contexts. [[HTTPS]]

[HDRP]: https://discussions.unity.com/t/early-access-to-the-new-webgpu-backend-in-unity-2023-3/933493/202
[HTTPS]: https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts#when_is_a_context_considered_secure
