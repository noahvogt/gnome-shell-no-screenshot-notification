# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=2.31.2
pkgrel=1
pkgdesc="The next generation GNOME Shell. Experimental, GNOME 3.0 version."
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('gnome-desktop>=2.29.92' 'gnome-python' 'gnome-menus>=2.29.92' 'mutter>=2.29.1' 'librsvg' 'gjs>=0.6')
makedepends=('intltool' 'pkgconfig' 'gnome-doc-utils>=0.19.5' 'gir-repository')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
source=(http://ftp.gnome.org/pub/GNOME/sources/gnome-shell/2.31/gnome-shell-$pkgver.tar.bz2)

build() {
  cd ${srcdir}/${pkgname}-${pkgver}
  ./configure --prefix=/usr --sysconfdir=/etc \
   	--localstatedir=/var --disable-static || return 1
  make || return 1
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install || return 1

  install -m755 -d "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain gnome-shell ${pkgdir}/etc/gconf/schemas/*.schemas || return 1
  rm -f ${pkgdir}/etc/gconf/schemas/*.schemas
}

sha1sums=('9df6117f9d3b9f9056e71f801cb1cefe66569ee9')
