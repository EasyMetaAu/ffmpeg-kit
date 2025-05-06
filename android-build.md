# 详细解析 FFmpeg-Kit 构建命令

```bash
./android.sh --lts --disable-x86 --disable-x86-64 --enable-gpl --enable-openssl --enable-android-media-codec --enable-libvpx --enable-libvorbis --enable-opus --enable-opencore-amr --enable-soxr --enable-x264 --enable-x265 --enable-libwebp
```

## 基础参数

- **./android.sh**: FFmpeg-Kit 的 Android 平台构建脚本
- **--lts**: 构建长期支持版本，兼容 API 16+ 的 Android 设备（Android 4.1 Jelly Bean 及更高版本）

## 架构设置

- **--disable-x86**: 不构建 x86 架构的库
- **--disable-x86-64**: 不构建 x86-64 架构的库

默认情况下，FFmpeg-Kit 会构建五种架构（armeabi-v7a, armeabi-v7a-neon, arm64-v8a, x86, x86_64）。通过禁用 x86 和 x86_64，我们只构建 ARM 架构版本，这覆盖了绝大多数 Android 设备，同时减少了构建时间和最终 AAR 文件的大小。

## 许可证选项

- **--enable-gpl**: 允许使用 GPL 许可证的库。这是使用 x264 和 x265 等库的必要条件，但会使最终产品受 GPLv3 许可证约束。

## 功能库

### 安全性和硬件加速

- **--enable-openssl**: 添加 OpenSSL 支持，用于处理加密流和网络协议（如 HTTPS）
- **--enable-android-media-codec**: 启用 Android 原生 MediaCodec 支持，提供硬件加速编解码，大幅提升性能

### 视频处理核心库

- **--enable-libvpx**: 添加 VP8/VP9 视频编解码器支持（Google 开发，用于 WebM 格式）
- **--enable-x264**: 添加 H.264/AVC 视频编解码支持（最广泛使用的视频编码格式之一）
- **--enable-x265**: 添加 H.265/HEVC 视频编解码支持（比 H.264 提供更好的压缩率）

### 音频处理库

- **--enable-libvorbis**: 添加 Ogg Vorbis 音频编解码器支持（MP3 的开源替代品）
- **--enable-opus**: 添加 Opus 音频编解码器支持（专为网络传输优化的高质量音频编码）
- **--enable-opencore-amr**: 添加自适应多速率（AMR）音频编解码器支持（常用于语音内容）
- **--enable-soxr**: 添加 SoX 重采样库，提供高质量的音频采样率转换

### 图像处理库

- **--enable-libwebp**: 添加 WebP 图像格式支持，用于图像水印处理

## 功能与库的关系

1. **视频提取音频**: 基础 FFmpeg 功能 + android-media-codec (硬件加速)
2. **音频格式转换**: libvorbis, opus, opencore-amr 和 soxr
3. **视频压缩**: x264, x265, libvpx
4. **视频裁剪**: 基础 FFmpeg 功能
5. **图片水印**: 基础 FFmpeg 功能 + libwebp (如果使用 WebP 格式图片)

这个构建配置提供了一个功能全面、性能优化的 FFmpeg 库，专为您的移动应用程序需求定制，同时避免了包含不必要的库来减少最终的文件大小。
