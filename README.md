# media-kit (Custom Fork)

This repository is a fork of [Predidit/media-kit](https://github.com/Predidit/media-kit) (which is based on the original [media-kit/media-kit](https://github.com/media-kit/media-kit)) built upon the stable reference `11d02cb804b8faf944137834a1b0ac80880a4079`.

## Key Enhancements & Fixes

### 1. HLS Stream Skipping Fix
- **Extended Network Timeout:** Increased `network-timeout` from 5s to 30s to prevent premature HLS segment aborts during mobile data or network latency spikes.
- **Disabled Video Frame Dropping:** Set `framedrop=no` to prevent mpv from skipping video frames when encountering segment boundary PTS jumps or discontinuities.
- **Increased Readahead Buffer:** Set `demuxer-readahead-secs=30` (up from 1s default) to maintain a healthy segment buffer.
- **Better Retry Handling:** Increased `seg_max_retry=10` and enabled `http_persistent=1` to reliably fetch segments.

### 2. Audio / Video Desynchronization Fix
- **Mobile Audio Driver Autosync:** Set `autosync=30` to smoothly and continuously compensate for audio delay jitter on mobile audio backends (Android OpenSL ES and iOS AudioUnit).
- **Surface Reattachment Seek Guard:** Guarded `widListener` seek logic in `AndroidVideoController` so seeking is only performed when position is valid and greater than zero, preventing disruptive demuxer resets on stream start or live HLS playback.

### 3. Performance & Stutter Optimization
- **RAM-Only Demuxer Cache:** Disabled `cache-on-disk` (`cache-on-disk=no`) to avoid flash storage I/O bottlenecks, wear, and thermal throttling on mobile devices.

### Inherited Features from Predidit/media-kit
- Heuristic HLS ad-blocking via `hls_ad_filter` in custom FFmpeg/libmpv build.
- Windows native D3D11 zero-copy hardware decoding without ANGLE dependency (including ARM64).
- Linux bundled pre-built `libmpv2.so`.
- Linux triple-buffering Vulkan swapchain simulation to prevent X11/OpenGL driver glitches.
- Out-of-tree FFmpeg patches from `avbuild` for non-standard video streams.

---

## Usage in `pubspec.yaml`

```yaml
dependencies:
  media_kit:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./media_kit
  media_kit_video:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./media_kit_video
  media_kit_libs_video:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./libs/universal/media_kit_libs_video

dependency_overrides:
  media_kit:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./media_kit
  media_kit_video:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./media_kit_video
  media_kit_libs_video:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./libs/universal/media_kit_libs_video
  media_kit_libs_linux:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./libs/linux/media_kit_libs_linux
  media_kit_libs_ios_video:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./libs/ios/media_kit_libs_ios_video
  media_kit_libs_android_video:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./libs/android/media_kit_libs_android_video
  media_kit_libs_windows_video:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./libs/windows/media_kit_libs_windows_video
  media_kit_libs_macos_video:
    git:
      url: https://github.com/RyanYuuki/media-kit.git
      ref: main
      path: ./libs/macos/media_kit_libs_macos_video
```