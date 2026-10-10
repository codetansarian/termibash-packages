# termibash-packages

Package repository for TermiBash, featuring packages maintained by their respective authors.

This repository hosts the **binary package repository** (package index + package archives) used
by the **TermiBash** terminal application for Android (32-bit `armeabi-v7a` and
64-bit `arm64-v8a`). It is kept separate
from the application source code so that packages can evolve and grow independently of the app.

The archives distributed here are **unmodified (except where explicitly noted below) prebuilt
binaries** originally produced by the [Termux project](https://github.com/termux/termux-packages)
for the `arm` (armeabi-v7a) and `aarch64` (arm64-v8a) architectures. All packages remain the property of their respective
authors and are distributed under their own upstream licenses, listed in the table below and
included in full under [`LICENSES/`](./LICENSES).

## Repository layout

```
.
├── README.md              <- this file (license compliance + package table)
├── index.tsv              <- machine-readable package index (arm + aarch64 + all rows)
├── index-aarch64.tsv      <- machine-readable package index (64-bit aarch64 extensions)
├── LICENSES/              <- full text of every license used by any package here
└── pool/                  <- package archives (.tar.gz, paths relative to the terminal $PREFIX)
```

## `index.tsv` format

One package per line, 8 tab-separated columns (one row per package/architecture combination; `index-aarch64.tsv` repeats the `aarch64` rows):

| # | Column | Meaning |
|---|--------|---------|
| 1 | name | package name, as used by `pkg install <name>` |
| 2 | version | upstream package version |
| 3 | arch | `arm` (armeabi-v7a), `aarch64` (arm64-v8a) or `all` (architecture-independent) |
| 4 | filename | archive path relative to this repository root |
| 5 | sha256 | SHA-256 checksum of the archive |
| 6 | size | archive size in bytes |
| 7 | depends | comma-separated runtime dependency names, `-` if none |
| 8 | description | short upstream description |

The app verifies the SHA-256 checksum of every archive before extraction and resolves
`depends` recursively before installation.

## 64-bit (aarch64) support

The package index is **mixed**: `index.tsv` carries `arm` (armeabi-v7a), `aarch64`
(arm64-v8a) and architecture-independent (`all`) rows in one file, so a resolver only
needs to filter rows by the third column (the app's `pkg` does exactly that: rows whose
arch matches the device - or `all` - are visible, everything else is hidden).
`index-aarch64.tsv` is kept as a convenient aarch64-only view of the same rows.

Native aarch64 builds exist for the development-toolchain tier - the packages where a
64-bit address space is a genuine requirement (large compilation jobs, linkers, the Go,
Rust and Node.js toolchains, code formatters such as `biome`) - plus the full dependency
closures of the newer application packages (`ffmpeg`, `openssh`, `nmap`, `util-linux`,
`redis`, `vim`, `git-lfs` and their libraries). Interpreters and small tools that run
identically well in 32-bit mode (python, ruby, php, lua, wget, zip, make) stay
arm-only; archive filenames always encode the architecture (`_arm`, `_aarch64`, `_all`),
so all three sets coexist in one `pool/` directory.

## Packages (package index: 291 rows - 150 arm, 134 aarch64, 7 arch-independent)

| Package | Version | Arch | Description | License |
|---|---|---|---|---|
| biome | 2.5.15 | aarch64 | Format, lint, and more for JavaScript, TypeScript, and JSON | MIT License |
| brotli | 1.2.0 | arm + aarch64 | lossless compression algorithm and format (command line utility) | MIT License |
| c-ares | 1.34.8 | arm + aarch64 | Library for asynchronous DNS requests (including name resolves) | MIT License |
| ca-certificates | 1:2026.09.25 | all | Common CA certificates | Mozilla Public License 2.0 |
| capstone | 5.0.9 | arm | Lightweight multi-platform, multi-architecture disassembly framework | BSD License |
| clang | 21.1.8-3 | arm + aarch64 | C language frontend for LLVM | Apache License 2.0 / Apache-2.0 LLVM Exception |
| ffmpeg | 8.1.3 | arm + aarch64 | Tools and libraries to manipulate a wide range of multimedia formats and protocols | GNU GPL v3.0 |
| fftw | 3.3.11 | arm + aarch64 | Library for computing the Discrete Fourier Transform (DFT) in one or more dimensions | GNU GPL v2.0 |
| fontconfig | 2.18.3 | arm + aarch64 | Library for configuring and customizing font access | MIT License |
| freetype | 2.14.3 | arm + aarch64 | Software font engine capable of producing high-quality output | GNU GPL v2.0 |
| fribidi | 1.0.17 | arm + aarch64 | Implementation of the Unicode Bidirectional Algorithm | GNU LGPL v2.0 |
| game-music-emu | 0.6.5 | arm + aarch64 | A collection of video game music file emulators | GNU LGPL v2.1 |
| gdbm | 1.26-1 | arm + aarch64 | Library of database functions that use extensible hashing | GNU GPL v3.0 |
| giflib | 6.1.3 | arm + aarch64 | A library for reading and writing gif images | MIT License |
| git | 2.56.0 | arm + aarch64 | Fast, scalable, distributed revision control system | GNU GPL v2.0 |
| git-lfs | 3.8.0 | arm + aarch64 | Git extension for versioning large files | MIT License |
| glib | 2.90.1 | arm + aarch64 | Library providing core building blocks for libraries and applications written in C | GNU LGPL v2.1 |
| glslang | 16.6.0 | arm + aarch64 | OpenGL and OpenGL ES shader front end and validator | BSD License |
| golang | 3:1.27.1 | arm + aarch64 | Go programming language compiler | BSD 3-Clause |
| harfbuzz | 14.6.0 | arm + aarch64 | OpenType text shaping engine | MIT License |
| highway | 1.4.0-1 | arm + aarch64 | Performance-portable, length-agnostic SIMD with runtime dispatch | Apache License 2.0 / BSD 3-Clause |
| krb5 | 1.22.2 | arm + aarch64 | The Kerberos network authentication system | MIT License |
| ldns | 1.9.2 | arm + aarch64 | Library for simplifying DNS programming and supporting recent and experimental RFCs | BSD 3-Clause |
| less | 710 | arm + aarch64 | Terminal pager program used to view the contents of a text file one screen at a time | GNU GPL v3.0 / custom |
| libandroid-execinfo | 0.1-3 | arm + aarch64 | Shared library for the backtrace system function | BSD 2-Clause |
| libandroid-glob | 0.6-3 | arm + aarch64 | Shared library for the glob(3) system function | BSD 3-Clause |
| libandroid-posix-semaphore | 0.1-4 | arm + aarch64 | Shared library for the posix semaphore system function | MIT License |
| libandroid-shmem | 0.7 | arm + aarch64 | System V shared memory emulation on Android using ashmem | BSD 3-Clause |
| libandroid-stub | 30-1 | arm + aarch64 | Stub libandroid.so for non-Android certified environment | NCSA/OpenBSD License |
| libandroid-support | 29-1 | arm + aarch64 | Library extending the Android C library (Bionic) for additional multibyte, locale and math support | Apache License 2.0 / MIT License |
| libaom | 3.15.2 | arm + aarch64 | AV1 Video Codec Library | BSD 2-Clause |
| libass | 0.17.5 | arm + aarch64 | A portable library for SSA/ASS subtitles rendering | BSD License |
| libbluray | 1.5.1 | arm + aarch64 | An open-source library designed for Blu-Ray Discs playback for media players | GNU LGPL v2.1 |
| libbs2b | 3.1.0-2 | arm + aarch64 | Bauer stereophonic-to-binaural DSP | MIT License |
| libbz2 | 1.0.8-8 | arm + aarch64 | BZ2 format compression library | BSD License |
| libc++ | 30 | arm + aarch64 | C++ Standard Library | NCSA/OpenBSD License |
| libcairo | 1.18.6 | arm + aarch64 | Cairo 2D vector graphics library | GNU LGPL v2.1 |
| libcap-ng | 2:0.9.6 | arm + aarch64 | Library making programming with POSIX capabilities easier than traditional libcap | GNU LGPL v2.1 |
| libcompiler-rt | 21.1.8-3 | arm + aarch64 | Compiler runtime libraries for clang | Apache License 2.0 / Apache-2.0 LLVM Exception |
| libcrypt | 0.2-6 | arm + aarch64 | A crypt(3) implementation | BSD 2-Clause |
| libcurl | 8.22.0 | arm + aarch64 | Easy-to-use client-side URL transfer library | MIT License |
| libdav1d | 1.5.4 | arm + aarch64 | AV1 cross-platform decoder focused on speed and correctness | BSD 2-Clause |
| libdb | 18.1.40-6 | arm + aarch64 | The Berkeley DB embedded database system (library) | GNU AGPL v3.0 |
| libdrm | 2.4.134 | arm + aarch64 | Userspace interface to kernel DRM services | MIT License |
| libdvdnav | 7.0.0 | arm + aarch64 | A library that allows easy use of sophisticated DVD navigation features | GNU GPL v2.0 |
| libdvdread | 7.1.1 | arm + aarch64 | A library that allows easy use of sophisticated DVD navigation features | GNU GPL v2.0 |
| libedit | 20260512-3.1-0 | arm + aarch64 | Library providing line editing, history, and tokenization functions | BSD 3-Clause |
| libexpat | 2.9.0 | arm + aarch64 | XML parsing C library | MIT License |
| libffi | 3.8.0 | arm + aarch64 | Library providing a portable, high level programming interface to various calling conventions | MIT License |
| libflac | 1.5.0-1 | arm + aarch64 | FLAC (Free Lossless Audio Codec) library | GNU GPL v2.0 / GNU LGPL v2.1 / BSD 3-Clause |
| libgcrypt | 1.12.4 | arm | General purpose cryptographic library based on the code from GnuPG | GNU GPL v2.0 / GNU LGPL v2.1 / BSD 3-Clause / MIT License / Public Domain |
| libgmp | 6.3.0-2 | arm | Library for arbitrary precision arithmetic | GNU LGPL v3.0 |
| libgpg-error | 1.61 | arm | Small library that defines common error values for all GnuPG components | GNU LGPL v2.1 |
| libgraphite | 1.3.15 | arm + aarch64 | Font system for multiple languages | GNU LGPL v2.0 |
| libiconv | 1.19 | arm + aarch64 | An implementation of iconv() | GNU LGPL v2.1 / GNU GPL v3.0 |
| libicu | 78.3 | arm + aarch64 | International Components for Unicode library | custom |
| libidn2 | 2.3.8-1 | arm | Free software implementation of IDNA2008, Punycode and TR46 | GNU LGPL v3.0 / GNU GPL v2.0 / GNU GPL v3.0 |
| libjpeg-turbo | 3.2.0 | arm + aarch64 | Library for reading and writing JPEG image files | IJG (JPEG) License / BSD 3-Clause / ZLIB |
| libjxl | 0.12.0-1 | arm + aarch64 | JPEG XL image format reference implementation | BSD 3-Clause |
| libllvm | 21.1.8-3 | arm + aarch64 | Modular compiler and toolchain technologies library | Apache License 2.0 / NCSA/OpenBSD License |
| liblzma | 5.8.4 | arm + aarch64 | XZ-format compression library | GNU LGPL v2.1 / GNU GPL v2.0 / GNU GPL v3.0 |
| liblzo | 2.10-5 | arm + aarch64 | Portable lossless data compression library | GNU GPL v2.0 |
| libmp3lame | 4.0-1 | arm + aarch64 | High quality MPEG Audio Layer III (MP3) encoder | GNU LGPL v2.0 |
| libmpg123 | 1.33.7 | arm + aarch64 | Fast console MPEG Audio Player and decoder library | GNU LGPL v2.1 |
| libmysofa | 1.3.5 | arm + aarch64 | Reader for AES SOFA files to get better HRTFs | BSD 3-Clause |
| libnghttp2 | 1.70.0 | arm + aarch64 | nghttp HTTP 2.0 library | MIT License |
| libnghttp3 | 1.18.0 | arm + aarch64 | HTTP/3 library written in C | MIT License |
| libngtcp2 | 1.25.0 | arm + aarch64 | Implementation of IETF QUIC protocol | MIT License |
| libogg | 1.3.6-1 | arm + aarch64 | Library for working with the Ogg multimedia container format | BSD 3-Clause |
| libopencore-amr | 0.1.6-1 | arm + aarch64 | Open source implementation of the Adaptive Multi Rate (AMR) speech codec | Apache License 2.0 |
| libopenmpt | 0.8.9 | arm + aarch64 | Library to render tracker music formats to a PCM audio stream | BSD 3-Clause |
| libopus | 1.6.1 | arm + aarch64 | Reference implementation of the Opus codec | BSD 3-Clause |
| libpcap | 1.11.0 | arm + aarch64 | Library for network traffic capture | BSD 3-Clause |
| libpixman | 0.46.4-1 | arm + aarch64 | Low-level library for pixel manipulation | MIT License |
| libplacebo | 7.360.1 | arm + aarch64 | Reusable library for GPU-accelerated video/image rendering | GNU LGPL v2.1 |
| libpng | 1.6.59 | arm + aarch64 | Official PNG reference library | libpng License |
| librav1e | 0.8.1 | arm + aarch64 | An AV1 encoder library focused on speed and safety | BSD 2-Clause |
| libresolv-wrapper | 1.1.8 | arm + aarch64 | A wrapper for DNS name resolving or DNS faking | BSD 3-Clause |
| libsamplerate | 0.2.2-4 | arm + aarch64 | A library for performing sample rate conversion of audio data | BSD 2-Clause |
| libsmartcols | 2.42.4 | arm + aarch64 | Library for smart adaptive formatting of tabular data | BSD 3-Clause License |
| libsndfile | 1.2.2-3 | arm + aarch64 | Library for reading/writing audio files | GNU LGPL v2.1 |
| libsodium | 1.0.22-1 | arm + aarch64 | Network communication, cryptography and signaturing library | ISC License |
| libsoxr | 0.1.3-8 | arm + aarch64 | High quality, one-dimensional sample-rate conversion library | GNU LGPL v2.1 |
| libsqlite | 3.54.0 | arm + aarch64 | Library implementing a self-contained and transactional SQL database engine | Public Domain |
| libsrt | 1.5.7 | arm + aarch64 | Secure Reliable Transport (SRT) Protocol | Mozilla Public License 2.0 |
| libssh | 0.12.2 | arm + aarch64 | Tiny C SSH library | GNU LGPL v2.1 / BSD 2-Clause |
| libssh2 | 1.11.1-2 | arm + aarch64 | Client-side library implementing the SSH2 protocol | BSD 3-Clause |
| libtheora | 1.2.0-1 | arm + aarch64 | An open video codec developed by the Xiph.org | BSD License |
| libtiff | 4.7.2 | arm + aarch64 | Support for the Tag Image File Format (TIFF) for storing image data | custom |
| libudfread | 1.2.0 | arm + aarch64 | A library for reading UDF | GNU LGPL v2.1 |
| libunistring | 1.4.2 | arm | Library providing functions for manipulating Unicode strings | GNU LGPL v3.0 / GNU GPL v2.0 |
| libuuid | 2.42.4 | arm | Library for handling universally unique identifiers | BSD 3-Clause License |
| libv4l | 1.32.0-1 | arm + aarch64 | Linux libraries to handle media devices | GNU LGPL v2.1 |
| libvidstab | 1.1.2 | arm + aarch64 | video stabilization library | GNU GPL v2.0 |
| libvmaf | 3.2.1 | arm + aarch64 | A perceptual video quality assessment algorithm developed by Netflix | custom |
| libvo-amrwbenc | 0.1.3-2 | arm + aarch64 | VisualOn AMR-WB encoder library | Apache License 2.0 |
| libvorbis | 1.3.7-4 | arm + aarch64 | Library for using the Ogg Vorbis compressed audio format | BSD 3-Clause |
| libvpx | 1:1.17.0 | arm + aarch64 | VP8 & VP9 Codec SDK | BSD 3-Clause |
| libwayland | 1.26.0 | arm + aarch64 | Wayland protocol library | MIT License |
| libwebp | 1.6.0-rc1-0 | arm + aarch64 | Library to encode and decode images in WebP format | BSD 3-Clause |
| libx11 | 1.8.13-1 | arm + aarch64 | X11 client-side library | MIT License / X11 License |
| libx264 | 1:0.164.3191-1 | arm + aarch64 | Library for encoding video streams into the H.264/MPEG-4 AVC format | GNU GPL v2.0 |
| libx265 | 4.3 | arm + aarch64 | H.265/HEVC video stream encoder library | GNU GPL v2.0 |
| libxau | 1.0.12-2 | arm + aarch64 | X11 authorisation library | MIT License |
| libxcb | 1.17.0-1 | arm + aarch64 | X11 client-side library | MIT License |
| libxdmcp | 1.1.5-2 | arm + aarch64 | X11 Display Manager Control Protocol library | MIT License |
| libxext | 1.3.7 | arm + aarch64 | X11 miscellaneous extensions library | MIT License / HPND License / ISC License |
| libxml2 | 2.15.4-1 | arm + aarch64 | Library for parsing XML documents | MIT License |
| libxrender | 0.9.12-1 | arm + aarch64 | X Rendering Extension client library | MIT License |
| libxshmfence | 1.3.3-1 | arm + aarch64 | A library that exposes a event API on top of Linux futexes | HPND License |
| libxslt | 1.1.45-1 | arm | XSLT processing library | MIT License |
| libyaml | 0.2.5-5 | arm | LibYAML is a YAML 1.1 parser and emitter written in C | MIT License |
| libzimg | 3.0.6 | arm + aarch64 | Scaling, colorspace conversion, and dithering library | WTFPL |
| libzip | 1.12 | arm | Library for reading, creating, and modifying zip archives | BSD License |
| libzmq | 4.3.5-2 | arm + aarch64 | Fast messaging system built on sockets. C and C++ bindings. aka 0MQ, ZMQ. | GNU LGPL v2.0 |
| littlecms | 2.19.1 | arm + aarch64 | Color management library | MIT License |
| lld | 21.1.8-3 | arm + aarch64 | LLVM-based linker | Apache License 2.0 / Apache-2.0 LLVM Exception |
| llvm | 21.1.8-3 | arm + aarch64 | LLVM modular compiler and toolchain executables | Apache License 2.0 / Apache-2.0 LLVM Exception |
| lua54 | 5.4.9 | arm + aarch64 | Lua scripting language 5.4.x | MIT License |
| make | 4.4.1-1 | arm | Tool to control the generation of non-source files from source files | GNU GPL v3.0 |
| mesa-vulkan-icd-swrast | 26.2.4 | arm + aarch64 | Mesa's Swrast Vulkan ICD | MIT License |
| ncurses | 6.6.20260307+really6.5.20250830 | arm + aarch64 | Library for text-based user interfaces in a terminal-independent manner | MIT License |
| ncurses-ui-libs | 6.6.20260307+really6.5.20250830 | arm + aarch64 | Libraries for terminal user interfaces based on ncurses | MIT License |
| ndk-sysroot | 30 | arm + aarch64 | System header and library files from the Android NDK needed for compiling C programs | NCSA/OpenBSD License |
| nmap | 7.991 | arm + aarch64 | Utility for network discovery and security auditing | Nmap License |
| nodejs | 26.4.0-1 | arm + aarch64 | Open Source, cross-platform JavaScript runtime environment | MIT License |
| npm | 11.20.0 | all | The package manager for JavaScript | Artistic-License-2.0 |
| ocl-icd | 2.3.5 | arm + aarch64 | OpenCL ICD Loader | BSD 2-Clause |
| oniguruma | 6.9.10-1 | arm | Regular expressions library | BSD License |
| openssh | 10.6p1-1 | arm + aarch64 | Secure shell for logging into a remote machine | BSD License |
| openssh-sftp-server | 10.6p1-1 | arm + aarch64 | OpenSSH SFTP server subsystem | BSD License |
| openssl | 1:3.6.5 | arm + aarch64 | Library implementing the SSL and TLS protocols as well as general purpose cryptography functions | Apache License 2.0 |
| pcre2 | 10.49 | arm + aarch64 | Perl 5 compatible regular expression library | BSD 3-Clause |
| php | 8.5.1 | arm | Server-side, HTML-embedded scripting language | PHP License v3.01 |
| python | 3.14.6-1 | arm + aarch64 | Python 3 programming language intended to enable clear programs | custom |
| python-pip | 26.2.1 | all | The PyPA recommended tool for installing Python packages | MIT License |
| readline | 8.3.6 | arm + aarch64 | Library that allow users to edit command lines as they are typed in | GNU GPL v3.0 |
| redis | 1:8.10.2 | arm + aarch64 | In-memory data structure store used as a database, cache and message broker | GNU AGPL v3.0 |
| resolv-conf | 1.3 | arm + aarch64 | Resolver configuration file | Public Domain |
| rubberband | 4.0.0-1 | arm + aarch64 | An audio time-stretching and pitch-shifting library and utility program | GNU GPL v2.0 |
| ruby | 4.0.7 | arm | Dynamic programming language with a focus on simplicity and productivity | BSD 2-Clause |
| rust | 1.99.0 | arm + aarch64 | Systems programming language focused on safety, speed and concurrency | MIT License |
| rust-std-aarch64-linux-android | 1.99.0 | aarch64 | Component files for target aarch64-linux-android | Apache License 2.0 / MIT License |
| rust-std-armv7-linux-androideabi | 1.99.0 | arm | Component files for target armv7-linux-androideabi | Apache License 2.0 / MIT License |
| svt-av1 | 4.2.0 | arm + aarch64 | Scalable Video Technology for AV1 (SVT-AV1 Encoder and Decoder) | BSD 2-Clause Patent License |
| termux-auth | 1.5.0-1 | arm + aarch64 | Password authentication library and utility for Termux | GNU GPL v3.0 |
| tidy | 5.9.14-next-3 | arm | A tool to tidy down your HTML code to a clean style | MIT License |
| ttf-dejavu | 2.37-8 | all | Font family based on the Bitstream Vera Fonts with a wider range of characters | MIT License |
| util-linux | 2.42.4 | arm + aarch64 | Miscellaneous system utilities | GNU GPL v3.0 or later / GNU GPL v2.0 or later / GNU LGPL v2.1 or later / BSD 3-Clause / BSD License / ISC License |
| vim | 9.2.1150 | arm + aarch64 | Vi IMproved - enhanced vi editor | VIM License |
| vulkan-icd | 0.1-1 | all | A metapackage that provides Vulkan ICDs | Public Domain |
| vulkan-loader-generic | 1.4.365 | arm + aarch64 | Vulkan Loader | Apache License 2.0 |
| wget | 1.25.0-1 | arm | Commandline tool for retrieving files using HTTP, HTTPS and FTP | GNU GPL v3.0 |
| xorg-util-macros | 1.20.2 | all | X.Org Autotools macros | HPND License / MIT License |
| xorgproto | 2026.1 | all | X.Org X11 Protocol headers | MIT License |
| xvidcore | 1.3.7-1 | arm + aarch64 | High performance and high quality MPEG-4 library | GNU GPL v2.0 |
| zip | 3.0-7 | arm | Tools for working with zip files | BSD License |
| zlib | 1.3.2 | arm + aarch64 | Compression library implementing the deflate compression method found in gzip and PKZIP | ZLIB |
| zstd | 1.5.7-1 | arm + aarch64 | Zstandard compression | GNU GPL v2.0 |

## License compliance

Every package in `pool/` is redistributed under the terms of its own upstream license:

1. **Attribution** - the table above maps each package to its license as declared by the
   upstream packaging ([termux/termux-packages](https://github.com/termux/termux-packages),
   whose build recipes are themselves licensed Apache-2.0). Upstream homepages for every
   package are available in the Termux package metadata.
2. **Full license texts** - the complete text of each distinct license used by any package
   in this repository is included in the [`LICENSES/`](./LICENSES) directory (fetched from the
   [SPDX License List](https://spdx.org/licenses/), which is itself CC0-licensed data).
3. **Copyright notices preserved** - package archives keep upstream copyright notices,
   license files and documentation shipped by the original builds.
4. **Copyleft licenses** - packages under GPL/LGPL/MPL remain under those terms; corresponding
   source code is available from each upstream project (linked from the Termux build recipes at
   `https://github.com/termux/termux-packages/tree/master/packages/<package-name>`), and from
   the upstream homepages listed there.
5. **No warranty** - all packages are distributed in the hope that they will be useful,
   but WITHOUT ANY WARRANTY; without even the implied warranty of MERCHANTABILITY or
   FITNESS FOR A PARTICULAR PURPOSE. See the individual licenses for details.

### Distinct licenses in this repository

- AGPL-3.0-only
- Apache-2.0
- Apache-2.0, BSD 3-Clause
- Apache-2.0, LLVM-exception
- Apache-2.0, MIT
- Apache-2.0, NCSA
- Artistic-License-2.0
- BSD
- BSD 2-Clause
- BSD 3-Clause
- BSD-2-Clause-Patent
- BSD-3-Clause
- GPL-2.0
- GPL-2.0, LGPL-2.1, BSD 3-Clause
- GPL-2.0, LGPL-2.1, BSD 3-Clause, MIT, Public Domain
- GPL-3.0
- GPL-3.0, custom
- GPL-3.0-or-later, GPL-2.0-or-later, LGPL-2.1-or-later, BSD 3-Clause, BSD, ISC
- HPND
- HPND, MIT
- IJG, BSD 3-Clause, ZLIB
- ISC
- LGPL-2.0
- LGPL-2.1
- LGPL-2.1, BSD 2-Clause
- LGPL-2.1, GPL-2.0, GPL-3.0
- LGPL-2.1, GPL-3.0
- LGPL-3.0
- LGPL-3.0, GPL-2.0
- LGPL-3.0, GPL-2.0, GPL-3.0
- Libpng
- MIT
- MIT, HPND, ISC
- MIT, X11
- MPL-2.0
- NCSA
- Nmap License
- PHP-3.01
- Public Domain
- VIM License
- WTFPL
- ZLIB
- custom

## Packaging notes (explicit deviations from upstream)

- Archives are repacked from Termux `.deb` packages into gzip tarballs whose paths are
  relative to the terminal `$PREFIX` (`/data/data/<app-id>/files/usr`).
- Path prefixes baked into **text files** (shell shebangs, `pkg-config`/CMake metadata,
  interpreter configs) are rewritten from the Termux prefix to the Termibash prefix during
  the repack. Machine code is left untouched: the app ships an `LD_PRELOAD` shim that
  transparently maps the baked Termux paths to the Termibash prefix at runtime, so
  binaries keep working unmodified.
- `rust`: upstream `share/doc`, `share/man` and the standard libraries for Android targets
  other than the repository's own are removed (the `arm` index keeps only the
  `armv7-linux-androideabi` target std, the `aarch64` index keeps only
  `aarch64-linux-android`). This keeps each archive within hosting file-size limits and
  removes content that cannot be used on the respective devices.
- `lua54`: adds convenience symlinks `bin/lua -> lua5.4` and `bin/luac -> luac5.4`
  (upstream ships only version-named binaries).
- `biome`: not packaged by Termux; redistributed from the official Biome release
  (`biome-linux-arm64-musl`, a fully static aarch64 build that runs on Android) as a
  single `bin/biome` binary.
- Versions are pinned to the Termux stable repository at build time (2026-10).

## Acknowledgements

- The [Termux project](https://termux.dev) and its contributors, whose packaging work
  these binaries come from.
- All upstream projects listed in the package table above - the actual software and its
  authors.
