# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.8.3
pkgrel=3
pkgdesc="The next generation GNOME Shell"
arch=(i686 x86_64)
url="http://live.gnome.org/GnomeShell"
license=(GPL2)
depends=(accountsservice caribou evolution-data-server gcr gjs gnome-bluetooth gnome-menus
         gnome-session gnome-settings-daemon gnome-themes-standard gsettings-desktop-schemas
         libcanberra-pulse libcroco libgdm libsecret mutter network-manager-applet
         telepathy-logger telepathy-mission-control unzip)
makedepends=(intltool gtk-doc gnome-control-center)
optdepends=('gnome-control-center: System settings')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver::3}/$pkgname-$pkgver.tar.xz
        nm-libexecdir.patch
        git-fixes.patch)
sha256sums=('145724f21ead9dbe0c39d3ab468b39264c7aaf3f0fa5d152b295d905d8339c22'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607'
            '3e08176d55c000eb88660349a4325e8134f4def66ef83efc7f45f0fb34a06e1c')

prepare() {
  cd $pkgname-$pkgver

  # FS#30747 FS#32730 Problems due to libexecdir different from NM
  patch -Np1 -i ../nm-libexecdir.patch

  # Git fixes - up to 60225ef86d070a7999bd6cded43837ca87d3f24f
  patch -Np1 -i ../git-fixes.patch
}

build() {
  cd $pkgname-$pkgver
  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --sysconfdir=/etc \
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
}
