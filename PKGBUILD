# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.8.0.1
pkgrel=1
pkgdesc="The next generation GNOME Shell"
arch=(i686 x86_64)
url="http://live.gnome.org/GnomeShell"
license=(GPL2)
depends=(caribou evolution-data-server gjs gnome-bluetooth gnome-menus libcroco mutter
         telepathy-logger telepathy-mission-control unzip gdm network-manager-applet
         libsecret gcr)
makedepends=(intltool gtk-doc gnome-control-center)
optdepends=('gnome-control-center: System settings'
            'gnome-themes-standard: Default theme')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver::3}/$pkgname-$pkgver.tar.xz
        nm-libexecdir.patch)
sha256sums=('cf98c3d038704fd057489c696a6cb9e2ee2ae5a10db5b45ddeba59fb82d507b8'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607')

build() {
  cd $pkgname-$pkgver

  # FS#30747 FS#32730 Problems due to libexecdir different from NM
  patch -Np1 -i ../nm-libexecdir.patch

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
