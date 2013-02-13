# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.6.3
pkgrel=1
pkgdesc="The next generation GNOME Shell"
arch=(i686 x86_64)
url="http://live.gnome.org/GnomeShell"
license=(GPL2)
depends=(caribou evolution-data-server gjs gnome-bluetooth gnome-menus libcroco mutter
         telepathy-logger telepathy-mission-control unzip gdm gnome-screensaver)
makedepends=(intltool gnome-doc-utils docbook-xsl)
optdepends=('gnome-control-center: System settings'
            'gnome-themes-standard: Default theme')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.xz)
sha256sums=('05b2341a0f84835644881743873d3eaccaed12f00aa7b424d876780e81723db2')

build() {
  cd $pkgname-$pkgver
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
