# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=2.91.92
pkgrel=1
pkgdesc="The next generation GNOME Shell"
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('mutter' 'gconf' 'dconf' 'gjs' 'gnome-menus' 'gnome-desktop' 'libcroco' 'libcanberra' 'libpulse' 'telepathy-glib' 'polkit-gnome'
         'gobject-introspection' 'evolution-data-server' 'gnome-bluetooth' 'gstreamer0.10' 'telepathy-logger')
makedepends=('intltool' 'gnome-doc-utils')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.bz2)
sha256sums=('5fd17e4d28094dee36a364cf5084785ffee21ca6790267d24cf4d2477d48b2e6')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"
  ./configure --prefix=/usr --sysconfdir=/etc \
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
