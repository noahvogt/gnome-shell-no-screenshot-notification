# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.0.2
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
        bluetoothstatus-always-update-devices.patch
        shell-recorder-missing-XFree.patch)
sha256sums=('a44963877da895d9b9f1ea98617067c5e88a5c4b414c6ccf0fcbfacdeac7db95'
            'a35d5e5f9f781728070aecae3bfe329f49dadcd50ca2984e0fbdd2219825a0db'
            'f592752875085fceebdb27e65802e09c07edd7be57eec0da3edfcad5052be2ae'
            '070edd5e720c063be41c158f39b7ef62a0d4a7f547ca0d23216104d5428ff971')

build() {
  cd "${srcdir}/${pkgname}-${pkgver}"

  patch -Np1 -i "${srcdir}/arch.patch"
  patch -Np1 -i "${srcdir}/bluetoothstatus-always-update-devices.patch"
  patch -Np1 -i "${srcdir}/shell-recorder-missing-XFree.patch"

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
