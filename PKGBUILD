# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.1.91.1
pkgrel=2
pkgdesc="The next generation GNOME Shell"
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('mutter' 'gjs' 'libcroco' 'gnome-bluetooth' 'gnome-desktop' 'gnome-menus' 'libpulse' 'folks' 'telepathy-logger' 'networkmanager' 'caribou')
makedepends=('intltool' 'gnome-doc-utils')
optdepends=('network-manager-applet: shell integration for networkmanager')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/3.1/$pkgname-$pkgver.tar.xz)
sha256sums=('d0da24c87f6851811e89774bb271b66377e67b5373ca3f73cfda7fec827f3865')

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
