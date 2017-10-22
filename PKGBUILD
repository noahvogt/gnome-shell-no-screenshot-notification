# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.26.1+6+g78d58deb5
pkgrel=1
pkgdesc="The next generation GNOME Shell"
url="https://wiki.gnome.org/Projects/GnomeShell"
arch=(i686 x86_64)
license=(GPL2)
depends=(accountsservice caribou gcr gjs gnome-bluetooth gnome-menus upower
         gnome-session gnome-settings-daemon gnome-themes-standard gsettings-desktop-schemas
         libcanberra-pulse libcroco libgdm libsecret mutter nm-connection-editor
         unzip gstreamer)
makedepends=(gtk-doc gnome-control-center evolution-data-server gobject-introspection git meson)
optdepends=('gnome-control-center: System settings'
            'evolution-data-server: Evolution calendar integration')
groups=(gnome)
_commit=78d58deb5afbe4fc06319553d4ed975053589d00  # gnome-3-26
source=("git+https://git.gnome.org/browse/gnome-shell#commit=$_commit"
        "git+https://git.gnome.org/browse/libgnome-volume-control"
        "git+https://git.gnome.org/browse/gnome-shell-sass"
        nm-libexecdir.patch)
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'e5bb10ad2e5c3e0fde3d05babd1bfdda701e553e02d493f7e54cb7832ce7e607')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  mkdir build
  cd $pkgname

  # FS#30747 FS#32730 Problems due to libexecdir different from NM
  patch -Np1 -i ../nm-libexecdir.patch

  # Move the plugin to our custom epihpany-only dir
  sed -i "s/'mozilla'/'epiphany'/g" meson.build

  git submodule init
  git config --local submodule.src/gvc.url "$srcdir/libgnome-volume-control"
  git config --local submodule.data/theme/gnome-shell-sass.url "$srcdir/gnome-shell-sass"
  git submodule update
}
  
build() {
  cd build
  arch-meson ../$pkgname -Denable-documentation=true
  ninja
}

package() {
  cd build
  DESTDIR="$pkgdir" ninja install

  # Must exist; FS#37412
  mkdir "$pkgdir/usr/share/gnome-shell/modes"
}
