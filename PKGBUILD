# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.3.92
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
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.xz)
sha256sums=('1c66198324512742c3be6879d15294c41239941480a7669cd547d7f8106977fe')

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
