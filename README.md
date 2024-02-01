## MPV Builder with VVC

This is a collection of scripts to build mpv, ffmpeg, libass, and various dependencies more easily.

## Overview

As [VVC](https://wiki.x266.mov/docs/video/VVC) video has begun trickling into the Internet, it is a conundrum that H.266 is nearly impossible to play back alongside its common audio pairing, [xHE-AAC](https://wiki.x266.mov/docs/audio/AAC#xhe-aac)/USAC. While FFmpeg recently included a native `ffvvc` decoder, it is not nearly as performant as Fraunhofer's `vvdec`. It may be someday, but not right now. FDK-AAC is also necessary to play back xHE-AAC audio, and it is under a non-free license which means FFmpeg binaries compiled with xHE-AAC support cannot be redistributed.

To address these issues and more, this modified version of the mpv-build scripting suite adds FFmpeg compilation support for the following libraries:

- FDK-AAC
- vvdec
- dav1d
- libjxl

It is important to note that non-free software builds *are not to be redistributed*. I do not endorse illicit redistribution of non-free software compiled with this tool.

## Generic Instructions

Make sure `git` is installed. Also check that the dependencies listed in
the next section are installed.

Clone this repo:
```bash
git clone https://github.com/gianni-rosato/mpv-build-vvc
cd mpv-build
```

To get the sources and build them, run the command:

```bash
./rebuild -j4
```

The `-j4` asks it to use 4 parallel processes.

Note that this command implicitly does an update followed by a full cleanup
(even if nothing changes), which is supposed to reduce possible problems with
incremental builds. You can do incremental builds by explicitly calling
`./build`. This can be faster on minor updates, but breaks sometimes, e.g.
the FFmpeg build system can sometimes be a bit glitchy.

Install mpv with:

```bash
sudo ./install
```

It is worth noting that the resulting mpv binary doesn't need to be installed. The binary `./mpv/build/mpv` can be used as-is. You can copy it to your `/usr/local/bin` manually, and even rename it if you'd like. Note that libass and ffmpeg will be statically linked with mpv when using the provided scripts, and no ffmpeg or libass libraries are/need to be installed. There are no required config or data files either.

*NOTE*: *If you are on debian, you may need to install meson from backports
in order to get a non-ancient version. Alternatively, you can install it from
PyPi.*

## Dependencies

Essential dependencies *(incomplete list)*:

- gcc or clang, yasm, git, meson, ninja
- cmake
- webp, libavif
- autoconf/autotools (for libass)
- X development headers (xlib, X extensions, vdpau, GL, Xv, ...)
- Audio output development headers (libasound, pulseaudio)
- fribidi, freetype, fontconfig development headers (for libass)
- libjpeg
- OpenSSL or GnuTLS development headers if you want to open https links
  (this is also needed to make youtube-dl interaction work)
- youtube-dl (at runtime) if you want to play Youtube videos directly
  (a builtin mpv script will call it)
- libx264/libmp3lame/libfdk-aac if you want to use encoding (you have to
  add these options explicitly to the ffmpeg options, as ffmpeg won't
  autodetect these libraries; see next section)

Note: most dependencies are optional and autodetected. If they're missing,
these features will be disabled silently. This includes some dependencies
which could be considered essential.

**For addition information, visit the [original `mpv-build` repository](https://github.com/mpv-player/mpv-build).**
