# libfprint-elanmoc2-git-0c00

Arch/CachyOS package integration for the ELAN `04f3:0c00` fingerprint reader enrollment fix.

This repository is based on the AUR package `libfprint-elanmoc2-git` and adds the device-specific enrollment patch for ELAN `04f3:0c00` readers.

## Tested Hardware

Confirmed working on:

```text
Vendor:      04f3
Product:     0c00
Product Name: ELAN:ARM-M4
bcdDevice:   2.81 / 0281
```

Test system:

```text
Distribution: CachyOS / Arch Linux
Desktop:      KDE Plasma
Login Manager: Plasma Login Manager (plasmalogin)
libfprint:    1.94.0+372+g11f0316
```

Confirmed working:

* Fingerprint enrollment
* `fprintd-verify`
* KDE fingerprint configuration
* Plasma Login Manager fingerprint login at initial startup

## Problem

With the standard `libfprint-elanmoc2-git` driver, the `04f3:0c00` reader can appear to enroll successfully but subsequently fail verification:

```text
verify-no-match
```

The `04f3:0c00` firmware expects a different enrollment command length than other ELAN MOC2 devices.

This patch selects the correct enrollment command specifically for `04f3:0c00` while preserving the existing behavior for other supported devices.

## Installation

Clone this repository:

```bash
git clone https://github.com/nopcarrier/libfprint-elanmoc2-git-0c00.git
cd libfprint-elanmoc2-git-0c00
```

Build and install:

```bash
makepkg -si
```

## Important: Re-enroll Your Fingerprint

Fingerprints enrolled using the unpatched driver should be deleted and enrolled again after installing this package.

You can use your desktop environment's fingerprint settings or:

```bash
fprintd-delete "$USER"
fprintd-enroll
```

Then verify:

```bash
fprintd-verify
```

A successful test should return:

```text
verify-match
```

## Plasma Login Manager

For KDE Plasma Login Manager (`plasmalogin`), fingerprint authentication can be enabled through PAM.

The relevant section of `/etc/pam.d/plasmalogin` should include:

```text
auth        sufficient  pam_fprintd.so
auth        include     system-login
```

Keep normal password authentication enabled as a fallback.

Depending on the Plasma Login Manager version, the greeter may not visibly indicate that it is waiting for a fingerprint. On the tested CachyOS system, submitting the login attempt activates `pam_fprintd`, after which the enrolled fingerprint can authenticate successfully.

## Patch Credit

The actual ELAN `04f3:0c00` protocol/enrollment fix is **not my original work**.

Original patch:

* Author: `bassemabdelbaset`
* Repository: https://github.com/bassemabdelbaset/libfprint-elan-04f3-0c00-ubuntu
* Original commit: `ec288f9f4f09801a74de30a0bbb364a382c2b11f`

This repository packages that fix for Arch Linux / CachyOS using the existing `libfprint-elanmoc2-git` AUR package structure.

## Acknowledgements

Thanks to:

* Davide Depau for the experimental `elanmoc2` libfprint driver
* `bassemabdelbaset` for the `04f3:0c00` enrollment fix
* The libfprint and Arch Linux communities

## Disclaimer

This driver and patch are experimental.

Always keep password authentication available as a fallback when modifying PAM or fingerprint authentication settings.
