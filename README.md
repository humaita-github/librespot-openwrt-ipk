# librespot-openwrt-ipk

This repository provides IPKs of [librespot](https://github.com/librespot-org/librespot) for OpenWrt. This work is based on [librespot-openwrt](https://github.com/izer-xyz/librespot-openwrt/).

# About librespot

> librespot is an open source client library for Spotify. It enables applications to use Spotify's service to control and play music via various backends, and to act as a Spotify Connect receiver. It is an alternative to the official and now deprecated closed-source libspotify. Additionally, it will provide extra features which are not available in the official library.

# Supported targets

We currently provide IPKs for the following Rust targets / OpenWrt package architectures:
* __aarch64-unknown-linux-musl__: aarch64_cortex-a53, aarch64_cortex-a72, aarch64_cortex-a76, aarch64_generic
* __arm*-unknown-linux-musl*__: arm_arm1176jzf-s_vfp, arm_arm926ej-s, arm_cortex-a15_neon-vfpv4, arm_cortex-a5_vfpv4, arm_cortex-a7, arm_cortex-a7_neon-vfpv4, arm_cortex-a8_vfpv3, arm_cortex-a9, arm_cortex-a9_neon, arm_cortex-a9_vfpv3-d16, arm_fa526, arm_xscale
* __loongarch64-unknown-linux-musl__: loongarch64_generic 
* __mips*-unknown-linux-musl__: mips64_mips64r2, mips64_octeonplus, mips_24kc, mips_mips32, mipsel_24kc, mipsel_24kc_24kf, mipsel_74kc, mipsel_mips32
* __powerpc*-unknown-linux-musl__: powerpc64_e5500, powerpc_464fp, powerpc_8548
* __riscv64gc-unknown-linux-musl__: riscv64_riscv64
* __x86_64-unknown-linux-musl__: x86_64

The following OpenWrt architectures don't compile "due to unsupported architecture" error: 
* __arm*-unknown-linux-musl*__: arm_cortex-a7_vfpv4, armeb_xscale
* __mips*-unknown-linux-musl__: mips64el_mips64r2, mips_4kec

The following OpenWrt architectures don't compile due to missing SSE support for Ring: 
* __i*86-unknown-linux-musl__: i386_pentium-mmx, i386_pentium4

# Building locally
Here is an example on how to build for mipsel (ramips-mt7621). Check the OpenWrt SDK manual to make sure you have all depencies installed before starting.

```
# Select the OpenWrt target / OpenWrt SDK you want to use
SDK_LINK=https://downloads.openwrt.org/releases/24.10.0/targets/ramips/mt7621/openwrt-sdk-24.10.0-ramips-mt7621_gcc-13.3.0_musl.Linux-x86_64.tar.zst

# Download and uncompress the respective OpenWrt SDK
SDK_FILE=$(echo $SDK_LINK | awk -F / '{print $NF}')
SDK_NAME=$(echo $SDK_FILE | awk -F .tar '{print $1}')
wget $SDK_LINK
tar -axf $SDK_FILE
rm $SDK_FILE
cd $SDK_NAME/

# Install this repository
echo 'src-git librespot https://github.com/humaita-github/librespot-openwrt-ipk' >> feeds.conf.default
./scripts/feeds update -a
./scripts/feeds install librespot

# Compile
make defconfig
make -j1 V=s package/feeds/librespot/librespot/compile

# Find the location of the IPK
ls -la -R bin/ | grep librespot | grep ipk
```
