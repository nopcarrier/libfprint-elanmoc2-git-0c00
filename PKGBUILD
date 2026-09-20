# Maintainer: Davide Depau <davide@depau.eu>
# Contributor: Johnathon Clark <john.clark at cantab dot net>
# Contributor: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Tom Gundersen <teg@jklm.no>
# Contributor: Thomas Baechler <thomas@archlinux.org>

# This is a build of the experimental elanmoc2 driver by Davide Depau

pkgname=libfprint-elanmoc2-git
_pkgname=libfprint
pkgver=1.94.0+372+g11f0316
pkgrel=1
pkgdesc="Library for fingerprint readers with patches for the support of the ELAN 0C4C."
url="https://fprint.freedesktop.org/"
arch=(x86_64)
license=(LGPL)
depends=(libgusb pixman nss systemd libgudev)
makedepends=(git meson gtk-doc gobject-introspection glib2-devel)
checkdepends=(cairo)
conflicts=(libfprint)
provides=(libfprint=1.94.0 libfprint-2.so)
groups=(fprint)

# ELAN 04f3:0c00 enrollment fix by bassemabdelbaset
# Source: https://github.com/bassemabdelbaset/libfprint-elan-04f3-0c00-ubuntu
# Original commit: ec288f9f4f09801a74de30a0bbb364a382c2b11f

source=(
  "git+https://gitlab.freedesktop.org/Depau/libfprint.git#branch=elanmoc2"
  "0001-elanmoc2-fix-enroll-command-for-04f3-0c00.patch"
)

sha256sums=(
  'SKIP'
  '9c7ba55ef16a8f3d8885ad1d1b23f168c8a2f020a130e69e2f8b9cbae31d3e63'
)

pkgver() {
  cd $_pkgname
  git describe --tags | sed 's/^v//;s/^V_//;s/_/./g;s/-/+/g'
}

prepare() {
  cd $_pkgname
  git apply "$srcdir/0001-elanmoc2-fix-enroll-command-for-04f3-0c00.patch"
}

build() {
  arch-meson $_pkgname build
  meson compile -C build
}

package() {
  meson install -C build --destdir "$pkgdir"
}
