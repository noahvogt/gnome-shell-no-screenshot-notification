# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.22.0+4+g717c0ea
pkgrel=1
pkgdesc="The next generation GNOME Shell"
url="https://wiki.gnome.org/Projects/GnomeShell"
arch=(i686 x86_64)
license=(GPL2)
depends=(accountsservice caribou gcr gjs gnome-bluetooth gnome-menus upower
         gnome-session gnome-settings-daemon gnome-themes-standard gsettings-desktop-schemas
         libcanberra-pulse libcroco libgdm libsecret mutter nm-connection-editor
         telepathy-logger telepathy-mission-control unzip gstreamer)
makedepends=(intltool gtk-doc gnome-control-center evolution-data-server python gobject-introspection git gnome-common)
optdepends=('gnome-control-center: System settings'
            'evolution-data-server: Evolution calendar integration')
groups=(gnome)
_commit=717c0ea19f91bd149c43e92d2a2c80dbc5ba0b1a  # master
source=("git://git.gnome.org/gnome-shell#commit=$_commit"
        nm-libexecdir.patch)
sha256sums=('SKIP'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  cd $pkgname

  # FS#30747 FS#32730 Problems due to libexecdir different from NM
  patch -Np1 -i ../nm-libexecdir.patch

  NOCONFIGURE=1 ./autogen.sh
}
  
build() {
  cd $pkgname
  ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/gnome-shell \
      --localstatedir=/var --disable-static \
      --disable-schemas-compile --enable-gtk-doc

  # https://bugzilla.gnome.org/show_bug.cgi?id=655517
  sed -i -e 's/ -shared / -Wl,-O1,--as-needed\0/g' libtool

  make
}

package() {
  cd $pkgname
  make DESTDIR="$pkgdir" install

  # Must exist; FS#37412
  mkdir -p "$pkgdir/usr/share/gnome-shell/modes"
}
