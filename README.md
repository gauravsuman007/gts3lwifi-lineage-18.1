# LineageOS 18.1 (Android 11) for the Galaxy Tab S3 — SM-T820 `gts3lwifi`

Unofficial LineageOS 18.1 builds for the Wi-Fi Galaxy Tab S3, built **natively on an arm64
Linux host** (Apple-silicon Mac → OrbStack Debian VM, no x86 emulation), plus the kernel
source they correspond to.

| Branch / item | Content |
|---|---|
| `main` | this README, release notes, checksums |
| `kernel-v0.13` | kernel source (Linux 3.18.140, msm8996, awesometic v0.13) |
| Releases | flashable zip, `boot.img`, kernel `Image.gz-dtb`, `SHA256SUMS` |
| Build setup | [`lineageos-arm64-host-build`](https://github.com/gauravsuman007/lineageos-arm64-host-build) — scripts, patches, guide, debugging, optimization plan |

## Releases
* **v1.0 — first boot** (`lineage-18.1-20261120`) — [notes](release-notes/v1.0.md)
* **v2.0 — performance** (planned) — [notes draft](release-notes/v2.0-performance.md)

## How the images were built
1. Sources: LineageOS `lineage-18.1` manifest + awesometic's `gts3lwifi`, `gts3l-common`,
   kernel and vendor trees (`local_manifest.xml` in the build repo). Exact commits of all
   869 projects: `pinned-revisions.txt` in the build repo.
2. Host: Debian 11 arm64 VM, 7 vCPU, 11 GB RAM, clang/lld/llvm 11, OpenJDK 11.0.32, ccache.
   Every x86 host prebuilt replaced by a native arm64 tool; 29 source projects patched
   for arm64/glibc/bison/JDK differences (see the build repo README §3).
3. `breakfast gts3lwifi userdebug && m -j7 bacon`.
4. Kernel: `boot.img` contains `Image.gz-dtb` from `TARGET_PREBUILT_KERNEL` — awesometic's
   **prebuilt v0.13 binary** (sha256 `f027448b…`), whose source is branch `kernel-v0.13`.
   Building that binary ourselves from this source is part of the v2 plan (WS2).

## Install (TWRP, data wipe required when coming from another Android version)
1. Back up everything (`scripts/backup_tablet.sh` in the build repo does all 47 partitions
   + GPT over adb, sha256-verified).
2. In TWRP: format data, install `lineage-18.1-…-gts3lwifi.zip`, reboot.
3. Optional Google apps: MindTheGapps 11.0.0 arm64 (flash before first boot, or format data
   again afterwards).

Verify downloads with `shasum -a 256 -c SHA256SUMS`.

## Status
Boots in ~40 s; Wi-Fi, Bluetooth, audio, display (1536×2048), all 21 sensors, Play Store
work. Not yet: encryption, SELinux enforcing, Gallery2, RenderScript compile, Qualcomm perf
boosts — see the release notes.

## Credits
LineageOS; awesometic (gts3l device/kernel/vendor trees); OnePlus/Qualcomm/Samsung vendor
blobs are property of their owners.
