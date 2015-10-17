# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.18.1
pkgrel=2
pkgdesc="The next generation GNOME Shell"
arch=(i686 x86_64)
url="http://live.gnome.org/GnomeShell"
license=(GPL2)
depends=(accountsservice caribou gcr gjs gnome-bluetooth gnome-menus upower
         gnome-session gnome-settings-daemon gnome-themes-standard gsettings-desktop-schemas
         libcanberra-pulse libcroco libgdm libsecret mutter nm-connection-editor
         telepathy-logger telepathy-mission-control unzip gstreamer)
makedepends=(intltool gtk-doc gnome-control-center evolution-data-server python gobject-introspection)
optdepends=('gnome-control-center: System settings'
            'evolution-data-server: Evolution calendar integration')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver:0:4}/$pkgname-$pkgver.tar.xz
        nm-libexecdir.patch fs1.patch fs2.patch)
sha256sums=('14a15215b3e29a25b94f69c58a6565e3a8cb2259b1ca242b906af78172bf3845'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607'
            '0d2f143362ff47bfd3dcbb94d903b51b9a998f2ff97c6320f0e3fe3599089e34'
            '19f39002be34f035c7f5f8f20bad50ab38fb8ff0f023e91e832feecc54fcdc60')

prepare() {
  cd $pkgname-$pkgver

  # Fullscreen animation issues
  patch -Np1 -i ../fs1.patch
  patch -Np1 -i ../fs2.patch

  # FS#30747 FS#32730 Problems due to libexecdir different from NM
  patch -Np1 -i ../nm-libexecdir.patch
}

build() {
  cd $pkgname-$pkgver
  ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/gnome-shell \
      --localstatedir=/var --disable-static \
      --disable-schemas-compile

  # https://bugzilla.gnome.org/show_bug.cgi?id=655517
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install

  # Must exist; FS#37412
  mkdir -p "$pkgdir/usr/share/gnome-shell/modes"
}
