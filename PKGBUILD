# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.20.0
pkgrel=3
pkgdesc="The next generation GNOME Shell"
arch=(i686 x86_64)
url="https://wiki.gnome.org/Projects/GnomeShell"
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
source=(https://download.gnome.org/sources/$pkgname/${pkgver:0:4}/$pkgname-$pkgver.tar.xz
        nm-libexecdir.patch
	offscreen-memleak.patch
        0001-Initialize-framebuffer-objects-early-so-clutter-will.patch)
sha256sums=('ee69f461dd3d03caf788dfc64241275868ec0bcd1ef814f3cd2803c25796b888'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607'
            '38bf66da2d92dbb3eab90d36feba0b1af65fe476d2982989dccd799aec0125a6'
            '4dfd8a416fafdfa53ff07fd0433d5785f7bf434d3e576710879bfca5fe39c84f')

prepare() {
  cd $pkgname-$pkgver

  # FS#30747 FS#32730 Problems due to libexecdir different from NM
  patch -Np1 -i ../nm-libexecdir.patch

  # Fix memleak
  patch -Np1 -i ../offscreen-memleak.patch

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
