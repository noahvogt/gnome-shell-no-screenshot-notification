# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.8.2
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
        nm-libexecdir.patch popupmenu.patch)
sha256sums=('ffdf42d382d50cd756f1f51a31eaa6877edb51a08f0ca80b6e973f05072416df'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607'
            '7df2a128d12350fe8e349c6aa5e125eb5d90b05e0201a842d6f3e1c2683b351d')

prepare() {
  cd $pkgname-$pkgver

  # FS#30747 FS#32730 Problems due to libexecdir different from NM
  patch -Np1 -i ../nm-libexecdir.patch

  # FS#35326 (from gnome-3-8 branch)
  patch -Np1 -i ../popupmenu.patch
}

build() {
  cd $pkgname-$pkgver
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
