# Unity Web Player

There are already several presentations covering this feature (including mine!),
so this page focuses on minor tips that are not commonly mentioned.

## Local Testing

You can't simply open `index.html` directly in your browser to test a build,
as it requires an HTTP server to serve the content. A convenient way to test
locally is by using Python's built-in HTTP server:

```
$ python3 -m http.server 8080
```

However, this method does not support testing WebGPU builds, as they require 
a secure context (i.e., HTTPS).

## Mobile Browsers

You can easily detect whether a build is running on a desktop or mobile browser 
using `Application.isMobilePlatform`.

https://docs.unity3d.com/6000.0/Documentation/ScriptReference/Application-isMobilePlatform.html

## Video Player

The `VideoPlayer` component does not support video assets in Web builds. To
play videos, place the files in the `StreamingAssets` folder and access them 
via a URL at runtime.

Unity Play supports Web projects up to 500 MB in size[^1], making it a good
choice if your project includes large video files.

[^1]: This is mentioned in the WebGL Publisher package documentation. The actual 
quota is not clearly specified--I was able to upload a project over 500 MB
without any issues.

## WebGPU

- HDRP is not supported. [[HDRP]]
- WebGPU is only available in secure contexts. [[HTTPS]]

[HDRP]: https://discussions.unity.com/t/early-access-to-the-new-webgpu-backend-in-unity-2023-3/933493/202  
[HTTPS]: https://developer.mozilla.org/en-US/docs/Web/Security/Secure_Contexts#when_is_a_context_considered_secure
