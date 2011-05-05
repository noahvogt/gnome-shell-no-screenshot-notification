# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.0.1
pkgrel=2
pkgdesc="The next generation GNOME Shell"
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('mutter' 'gconf' 'dconf' 'gjs' 'gnome-menus' 'gnome-desktop' 'libcroco' 'libcanberra' 'libpulse' 'telepathy-glib' 'polkit-gnome'
         'gobject-introspection' 'evolution-data-server' 'gnome-bluetooth' 'gstreamer0.10' 'telepathy-logger')
makedepends=('intltool' 'gnome-doc-utils')
optdepends=('network-manager-applet: shell integration for networkmanager'
            'gnome-power-manager: shell integration for power management')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.bz2
        arch.patch
        network_fixes_up_to_5090a4ccce.patch)
sha256sums=('01f7ae942ba9687a5e67d62423843ed404d77b35f74acc212a5f391beed8e079'
            'a35d5e5f9f781728070aecae3bfe329f49dadcd50ca2984e0fbdd2219825a0db'
            '01bf41483d5d8935ed2dd6294ee04024f2d9bcb2ef13276b07331e485965c822')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  patch -Np1 -i "${srcdir}/arch.patch"
  patch -Np1 -i "${srcdir}/network_fixes_up_to_5090a4ccce.patch"

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
