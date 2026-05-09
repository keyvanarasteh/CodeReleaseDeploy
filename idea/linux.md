# Linux Ecosystem

## Overview
This document covers packaging and distributing applications for various Linux distributions.

## Snap (Snapcraft)
1. **Snapcraft.yaml**: Configuring plugs, slots, and base environments.
2. **Store Login**: Generating a `SNAPCRAFT_STORE_CREDENTIALS` macaroon to authenticate CI environments without a browser.
3. **Multi-Arch**: Using QEMU and LXD to build for `amd64` and `arm64`.

## Flatpak
1. **Manifest**: Writing JSON/YAML manifests.
2. **Flathub**: Submission guidelines and runtime dependencies.

## AppImage
1. **AppRun \u0026 .desktop**: Structuring the AppDir.
2. **linuxdeploy**: Automating the bundling of shared libraries into a portable executable.

## DEB / RPM
1. **FPM**: Using Effing Package Management to streamline `.deb` and `.rpm` generation.
2. **APT / YUM Repositories**: Setting up custom PPA or repository hosting for direct updates.
