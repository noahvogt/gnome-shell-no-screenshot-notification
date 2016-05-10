# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.20.2
pkgrel=1
pkgdesc="The next generation GNOME Shell"
url="https://wiki.gnome.org/Projects/GnomeShell"
arch=(i686 x86_64)
license=(GPL2)
depends=(accountsservice caribou gcr gjs gnome-bluetooth gnome-menus upower
         gnome-session gnome-settings-daemon gnome-themes-standard gsettings-desktop-schemas
         libcanberra-pulse libcroco libgdm libsecret mutter nm-connection-editor
         telepathy-logger telepathy-mission-control unzip gstreamer)
makedepends=(intltool gtk-doc gnome-control-center evolution-data-server python gobject-introspection)
optdepends=('gnome-control-center: System settings'
            'evolution-data-server: Evolution calendar integration')
groups=(gnome)
source=(https://download.gnome.org/sources/$pkgname/${pkgver:0:4}/$pkgname-$pkgver.tar.xz
        nm-libexecdir.patch
        0001-Initialize-framebuffer-objects-early-so-clutter-will.patch)
sha256sums=('eaff6b177cc5bab16b252c45393a6c9305ad3837a288e738388c7b4d4bae13cd'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607'
            '4dfd8a416fafdfa53ff07fd0433d5785f7bf434d3e576710879bfca5fe39c84f')

prepare() {
  cd $pkgname-$pkgver

  # FS#30747 FS#32730 Problems due to libexecdir different from NM
  patch -Np1 -i ../nm-libexecdir.patch

  # Catch FBO allocation failure
  patch -Np1 -i ../0001-Initialize-framebuffer-objects-early-so-clutter-will.patch
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
