# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.2.1
pkgrel=1
pkgdesc="The next generation GNOME Shell"
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('mutter' 'gjs' 'libcroco' 'gnome-bluetooth' 'gnome-desktop' 'gnome-menus' 'libpulse' 'folks' 'telepathy-logger' 'networkmanager' 'caribou' 'nautilus' 'telepathy-mission-control')
makedepends=('intltool' 'gnome-doc-utils')
optdepends=('network-manager-applet: shell integration for networkmanager')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/3.2/$pkgname-$pkgver.tar.xz)
sha256sums=('ac05e3133bd279c84812d8d3d8e362f8b33563cdc3e7a6aefa9c0b1c2aeab0eb')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/gnome-shell \
      --localstatedir=/var --disable-static \
      --disable-schemas-compile
  make
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain gnome-shell ${pkgdir}/etc/gconf/schemas/*.schemas
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}
