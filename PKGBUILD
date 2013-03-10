# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.6.3.1
pkgrel=3
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
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver::3}/$pkgname-$pkgver.tar.xz
        main-Dont-mess-up-the-modal-stack-when-the-focus-a.patch nm-libexecdir.patch)
sha256sums=('4e0328d43ac443e7cc0c43bb67895112643952f14cd20fff1109c6cc5849d603'
            '968245e7db1c6921627cf0fbce4e4504cffbdb24898f834769a23a254ed6e125'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607')

build() {
  cd $pkgname-$pkgver

  # FS#32410
  patch -Np1 -i ../main-Dont-mess-up-the-modal-stack-when-the-focus-a.patch

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
