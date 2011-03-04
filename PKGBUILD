# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=2.91.90
pkgrel=2
pkgdesc="The next generation GNOME Shell"
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('mutter' 'gconf' 'gjs' 'gnome-menus' 'gnome-desktop' 'libcroco'
         'libcanberra' 'libpulse' 'telepathy-glib' 'polkit-gnome'
         'gobject-introspection' 'evolution-data-server' 'dbus-python')
makedepends=('intltool' 'gnome-doc-utils' 'bluez')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.bz2)
sha256sums=('0cb01d1b9cdd060883a88fab9377b8c4498ac24a967c5135e6ab6c05f598e332')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc \
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


