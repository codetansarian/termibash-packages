# termibash-packages

Package repository for TermiBash, featuring packages maintained by their respective authors.

This repository hosts the **binary package repository** (package index + package archives) used
by the **TermiBash** terminal application for Android (32-bit / `armeabi-v7a`). It is kept separate
from the application source code so that packages can evolve and grow independently of the app.

The archives distributed here are **unmodified (except where explicitly noted below) prebuilt
binaries** originally produced by the [Termux project](https://github.com/termux/termux-packages)
for the `arm` (armeabi-v7a) architecture. All packages remain the property of their respective
authors and are distributed under their own upstream licenses, listed in the table below and
included in full under [`LICENSES/`](./LICENSES).

## Repository layout

```
.
├── README.md              <- this file (license compliance + package table)
├── index.tsv              <- machine-readable package index (32-bit arm + arch-independent)
├── index-aarch64.tsv      <- machine-readable package index (64-bit aarch64 extensions)
├── LICENSES/              <- full text of every license used by any package here
└── pool/                  <- package archives (.tar.gz, paths relative to the terminal $PREFIX)
```

## `index.tsv` format

One package per line, 8 tab-separated columns:

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

`index-aarch64.tsv` extends `index.tsv` with native **aarch64 (arm64-v8a)** builds for the
development-toolchain tier - the packages where a 64-bit address space is a genuine
requirement (large compilation jobs, linkers, the Go and Rust toolchains, the V8 runtime)
- plus the native library closures those 64-bit binaries link against. `git` is included
because `cargo` drives it for git dependencies.

Packages that run identically well in 32-bit mode (interpreters and small tools such as
python, ruby, php, lua, wget, zip, make) intentionally have **no** aarch64 duplicate, and
architecture-independent (`all`) packages are shared through `index.tsv` instead of being
duplicated. On a 64-bit device a resolver should prefer rows from `index-aarch64.tsv` and
fall back to `index.tsv` for everything else; archive filenames always encode the
architecture (`_arm`, `_aarch64`, `_all`), so both sets coexist in one `pool/` directory.

## Packages (32-bit arm index: 64 packages; aarch64 index: 34 packages)

| Package | Version | Description | License |
|---|---|---|---|
| c-ares | 1.34.8 | Library for asynchronous DNS requests (including name resolves) | MIT License |
| ca-certificates | 1:2026.09.25 | Common CA certificates | Mozilla Public License 2.0 |
| capstone | 5.0.9 | Lightweight multi-platform, multi-architecture disassembly framework | BSD 3-Clause License |
| clang | 21.1.8-3 | C language frontend for LLVM | Apache License 2.0 / Apache-2.0 LLVM Exception |
| gdbm | 1.26-1 | Library of database functions that use extensible hashing | GNU GPL v3.0 |
| git | 2.56.0 | Fast, scalable, distributed revision control system | GNU GPL v2.0 |
| golang | 3:1.27.1 | Go programming language compiler | BSD 3-Clause License |
| less | 710 | Terminal pager program used to view the contents of a text file one screen at a time | GNU GPL v3.0 / Python Software Foundation License (PSF-2.0) |
| libandroid-execinfo | 0.1-3 | Shared library for the backtrace system function | BSD 2-Clause License |
| libandroid-glob | 0.6-3 | Shared library for the glob(3) system function | BSD 3-Clause License |
| libandroid-posix-semaphore | 0.1-4 | Shared library for the posix semaphore system function | MIT License |
| libandroid-support | 29-1 | Library extending the Android C library (Bionic) for additional multibyte, locale and math support | Apache License 2.0 / MIT License |
| libbz2 | 1.0.8-8 | BZ2 format compression library | BSD 3-Clause License |
| libc++ | 30 | C++ Standard Library | NCSA/OpenBSD License |
| libcompiler-rt | 21.1.8-3 | Compiler runtime libraries for clang | Apache License 2.0 / Apache-2.0 LLVM Exception |
| libcrypt | 0.2-6 | A crypt(3) implementation | BSD 2-Clause License |
| libcurl | 8.22.0 | Easy-to-use client-side URL transfer library | MIT License |
| libexpat | 2.9.0 | XML parsing C library | MIT License |
| libffi | 3.8.0 | Library providing a portable, high level programming interface to various calling conventions | MIT License |
| libgcrypt | 1.12.4 | General purpose cryptographic library based on the code from GnuPG | GNU GPL v2.0 / GNU LGPL v2.1 / BSD 3-Clause License / MIT License / Public Domain |
| libgmp | 6.3.0-2 | Library for arbitrary precision arithmetic | GNU LGPL v3.0 |
| libgpg-error | 1.61 | Small library that defines common error values for all GnuPG components | GNU LGPL v2.1 |
| libiconv | 1.19 | An implementation of iconv() | GNU LGPL v2.1 / GNU GPL v3.0 |
| libicu | 78.3 | International Components for Unicode library | Python Software Foundation License (PSF-2.0) |
| libidn2 | 2.3.8-1 | Free software implementation of IDNA2008, Punycode and TR46 | GNU LGPL v3.0 / GNU GPL v2.0 / GNU GPL v3.0 |
| libllvm | 21.1.8-3 | Modular compiler and toolchain technologies library | Apache License 2.0 / NCSA/OpenBSD License |
| liblzma | 5.8.4 | XZ-format compression library | GNU LGPL v2.1 / GNU GPL v2.0 / GNU GPL v3.0 |
| libnghttp2 | 1.70.0 | nghttp HTTP 2.0 library | MIT License |
| libnghttp3 | 1.18.0 | HTTP/3 library written in C | MIT License |
| libngtcp2 | 1.25.0 | Implementation of IETF QUIC protocol | MIT License |
| libresolv-wrapper | 1.1.8 | A wrapper for DNS name resolving or DNS faking | BSD 3-Clause License |
| libsqlite | 3.53.4 | Library implementing a self-contained and transactional SQL database engine | Public Domain |
| libssh2 | 1.11.1-2 | Client-side library implementing the SSH2 protocol | BSD 3-Clause License |
| libunistring | 1.4.2 | Library providing functions for manipulating Unicode strings | GNU LGPL v3.0 / GNU GPL v2.0 |
| libuuid | 2.42.4 | Library for handling universally unique identifiers | BSD 3-Clause License |
| libxml2 | 2.15.4-1 | Library for parsing XML documents | MIT License |
| libxslt | 1.1.45-1 | XSLT processing library | MIT License |
| libyaml | 0.2.5-5 | LibYAML is a YAML 1.1 parser and emitter written in C | MIT License |
| libzip | 1.12 | Library for reading, creating, and modifying zip archives | BSD 3-Clause License |
| lld | 21.1.8-3 | LLVM-based linker | Apache License 2.0 / Apache-2.0 LLVM Exception |
| llvm | 21.1.8-3 | LLVM modular compiler and toolchain executables | Apache License 2.0 / Apache-2.0 LLVM Exception |
| lua54 | 5.4.9 | Lua scripting language 5.4.x | MIT License |
| make | 4.4.1-1 | Tool to control the generation of non-source files from source files | GNU GPL v3.0 |
| ncurses | 6.6.20260307+really6.5.20250830 | Library for text-based user interfaces in a terminal-independent manner | MIT License |
| ncurses-ui-libs | 6.6.20260307+really6.5.20250830 | Libraries for terminal user interfaces based on ncurses | MIT License |
| ndk-sysroot | 30 | System header and library files from the Android NDK needed for compiling C programs | NCSA/OpenBSD License |
| nodejs | 26.4.0-1 | Open Source, cross-platform JavaScript runtime environment | MIT License |
| npm | 11.20.0 | The package manager for JavaScript | Artistic License 2.0 |
| oniguruma | 6.9.10-1 | Regular expressions library | BSD 3-Clause License |
| openssl | 1:3.6.5 | Library implementing the SSL and TLS protocols as well as general purpose cryptography functions | Apache License 2.0 |
| pcre2 | 10.49 | Perl 5 compatible regular expression library | BSD 3-Clause License |
| php | 8.5.1 | Server-side, HTML-embedded scripting language | PHP License v3.01 |
| python | 3.14.6-1 | Python 3 programming language intended to enable clear programs | Python Software Foundation License (PSF-2.0) |
| python-pip | 26.2.1 | The PyPA recommended tool for installing Python packages | MIT License |
| readline | 8.3.6 | Library that allow users to edit command lines as they are typed in | GNU GPL v3.0 |
| resolv-conf | 1.3 | Resolver configuration file | Public Domain |
| ruby | 4.0.7 | Dynamic programming language with a focus on simplicity and productivity | BSD 2-Clause License |
| rust | 1.99.0 | Systems programming language focused on safety, speed and concurrency | MIT License |
| rust-std-armv7-linux-androideabi | 1.99.0 | Component files for target armv7-linux-androideabi | Apache License 2.0 / MIT License |
| tidy | 5.9.14-next-3 | A tool to tidy down your HTML code to a clean style | MIT License |
| wget | 1.25.0-1 | Commandline tool for retrieving files using HTTP, HTTPS and FTP | GNU GPL v3.0 |
| zip | 3.0-7 | Tools for working with zip files | BSD 3-Clause License |
| zlib | 1.3.2 | Compression library implementing the deflate compression method found in gzip and PKZIP | zlib License |
| zstd | 1.5.7-1 | Zstandard compression | GNU GPL v2.0 |

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

- Apache-2.0
- Apache-2.0 OR MIT
- Apache-2.0 WITH LLVM-exception
- Apache-2.0, MIT
- Apache-2.0, NCSA
- Artistic-License-2.0
- BSD
- BSD 2-Clause
- BSD 3-Clause
- BSD-3-Clause
- GPL-2.0
- GPL-2.0, LGPL-2.1, BSD 3-Clause, MIT, Public Domain
- GPL-3.0
- GPL-3.0, custom
- LGPL-2.1
- LGPL-2.1, GPL-2.0, GPL-3.0
- LGPL-2.1, GPL-3.0
- LGPL-3.0
- LGPL-3.0, GPL-2.0
- LGPL-3.0, GPL-2.0, GPL-3.0
- MIT
- MPL-2.0
- NCSA
- PHP-3.01
- Public Domain
- ZLIB
- custom

## Packaging notes (explicit deviations from upstream)

- Archives are repacked from Termux `.deb` packages into gzip tarballs whose paths are
  relative to the terminal `$PREFIX` (`/data/data/<app-id>/files/usr`); no package content
  is modified by the repack.
- `rust`: upstream `share/doc`, `share/man` and the standard libraries for Android targets
  other than the repository's own are removed (the `arm` index keeps only the
  `armv7-linux-androideabi` target std, the `aarch64` index keeps only
  `aarch64-linux-android`). This keeps each archive within hosting file-size limits and
  removes content that cannot be used on the respective devices.
- `lua54`: adds convenience symlinks `bin/lua -> lua5.4` and `bin/luac -> luac5.4`
  (upstream ships only version-named binaries).
- Versions are pinned to the Termux stable repository at build time (2026-10).

## Acknowledgements

- The [Termux project](https://termux.dev) and its contributors, whose packaging work
  these binaries come from.
- All upstream projects listed in the package table above - the actual software and its
  authors.
