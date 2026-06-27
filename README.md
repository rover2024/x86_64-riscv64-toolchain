# x86_64 Toolchain for RISC-V64 Hosts

A Canadian-cross GCC toolchain that **runs on a riscv64 host** and **produces x86_64 binaries**.

It exists for [Lorelei](https://github.com/rover2024/lorelei): on a riscv64 host the guest
(x86_64) thunk libraries must be compiled by a real GCC, because the thunk ABI reserves a
register with `-ffixed-r11`, which Clang does not support on x86_64. No distribution ships an
x86_64 cross GCC for a riscv64 host, so this toolchain fills that gap.

## Details

- build / host / target: `x86_64` / `riscv64-linux-gnu` / `x86_64-unknown-linux-gnu`
- GCC 11.4.0, glibc 2.35, binutils 2.40 (the same GCC 11 used on the x86_64 and aarch64 hosts)
- Built with crosstool-NG 1.26.0; the exact `.config` is attached to each release

## Use

```bash
curl -fsSL -o tc.tar.xz \
  https://github.com/rover2024/x86_64-riscv64-toolchain/releases/download/<tag>/<file>.tar.xz
tar -xf tc.tar.xz -C /opt
export PATH=/opt/x86_64-unknown-linux-gnu/bin:$PATH
x86_64-unknown-linux-gnu-gcc --version
```
