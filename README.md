# x86_64 Toolchain for RISC-V64 Hosts

Prebuilt toolchains for [Lorelei](https://github.com/rover2024/lorelei). Two things live here (the
repository name predates the second):

- a **Canadian-cross GCC** that runs on a riscv64 host and produces x86_64 binaries
- a **self-contained Clang/LLVM 20.1.8** for x86_64, aarch64 and riscv64 hosts, used by the Lorelei
  devkit

## Canadian-Cross GCC

Runs on a riscv64 host, targets `x86_64-unknown-linux-gnu`. On a riscv64 host the guest (x86_64) thunk
libraries need a real GCC, and no distribution ships an x86_64 cross GCC for a riscv64 host, so this
toolchain fills the gap.

- build / host / target: `x86_64` / `riscv64-linux-gnu` / `x86_64-unknown-linux-gnu`
- GCC 11.4.0, glibc 2.35, binutils 2.40 (the same GCC 11 used on the x86_64 and aarch64 hosts)
- built with crosstool-NG 1.26.0; the exact `.config` is attached to each release

```bash
curl -fsSL -o tc.tar.xz \
  https://github.com/rover2024/x86_64-riscv64-toolchain/releases/download/<tag>/<file>.tar.xz
tar -xf tc.tar.xz -C /opt
export PATH=/opt/x86_64-unknown-linux-gnu/bin:$PATH
x86_64-unknown-linux-gnu-gcc --version
```

## Self-Contained Clang/LLVM

Clang/LLVM 20.1.8 built with libedit, libxml2, libffi, libtinfo, zlib and zstd all off, so `libLLVM.so`
depends only on glibc (>= 2.38) and libstdc++. The distribution's LLVM debs pull `libedit.so.2` and fail
to start on Fedora and Arch, which ship `libedit.so.0`; this build runs on any distribution at or above
the glibc floor. The Lorelei distribution links LoreTLC against it and bundles its runtime `.so`s plus
the clang driver into the devkit.

- backends X86, AArch64, RISCV (each host compiles the x86_64 guest thunk and its own-arch host thunk)
- one asset per host arch: `clang20.1.8-glibc2.38-<host>host.tar.xz`

```bash
curl -fsSL -o clang.tar.xz \
  https://github.com/rover2024/x86_64-riscv64-toolchain/releases/download/clang-20.1.8-glibc-2.38/clang20.1.8-glibc2.38-x86_64host.tar.xz
tar -xf clang.tar.xz -C /opt
export PATH=/opt/clang-20.1.8-x86_64host/bin:$PATH
clang --version
```
