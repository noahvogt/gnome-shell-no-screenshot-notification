# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.4.2
pkgrel=1
pkgdesc="The next generation GNOME Shell"
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('caribou' 'folks' 'gcr' 'gjs' 'gnome-bluetooth' 'gnome-desktop' 'gnome-menus' 'libcroco' 'libpulse' 'mutter' 'nautilus' 'networkmanager' 'telepathy-logger' 'telepathy-mission-control' 'unzip')
makedepends=('intltool' 'gnome-doc-utils')
optdepends=('network-manager-applet: shell integration for networkmanager')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.xz
        recorder.patch)
sha256sums=('3807f7882968d032f8f5c64b0e0af51c0d016f2e1c4fd1576203c9350e412720'
            'b00589e867c0ae63b47982145cb4ab366afec84a568e66867f51fa8da13027f1')

build() {
  cd $pkgname-$pkgver
  patch -Np1 -i ../recorder.patch
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/gnome-shell \
      --localstatedir=/var --disable-static \
      --disable-schemas-compile
  make
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install
}
