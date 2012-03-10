# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.3.90
pkgrel=1
pkgdesc="The next generation GNOME Shell"
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('mutter' 'gjs' 'libcroco' 'gnome-bluetooth' 'gnome-desktop' 'gnome-menus' 'libpulse' 'folks' 'telepathy-logger' 'networkmanager' 'caribou' 'nautilus' 'telepathy-mission-control' 'unzip')
makedepends=('intltool' 'gnome-doc-utils')
optdepends=('network-manager-applet: shell integration for networkmanager')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.xz)
sha256sums=('8411932ee860415e37ef2da55536538430dc168dbc55fe7790d6f3bc15884532')

build() {
  cd "$pkgname-$pkgver"

  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/gnome-shell \
      --localstatedir=/var --disable-static \
      --disable-schemas-compile
  make
}

package() {
  cd "$pkgname-$pkgver"
  make DESTDIR="$pkgdir" install
}
