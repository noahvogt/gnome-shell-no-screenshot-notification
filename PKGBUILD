# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.26.2+9+ga3736d3a3
pkgrel=1
pkgdesc="The next generation GNOME Shell"
url="https://wiki.gnome.org/Projects/GnomeShell"
arch=(x86_64)
license=(GPL2)
depends=(accountsservice caribou gcr gjs gnome-bluetooth gnome-menus upower
         gnome-session gnome-settings-daemon gnome-themes-standard gsettings-desktop-schemas
         libcanberra-pulse libcroco libgdm libsecret mutter nm-connection-editor
         unzip gstreamer)
makedepends=(gtk-doc gnome-control-center evolution-data-server gobject-introspection git meson)
optdepends=('gnome-control-center: System settings'
            'evolution-data-server: Evolution calendar integration')
groups=(gnome)
_commit=a3736d3a3951af8d5f67c3d0a961e5df9be7825f  # gnome-3-26
source=("git+https://git.gnome.org/browse/gnome-shell#commit=$_commit"
        "git+https://git.gnome.org/browse/libgnome-volume-control"
        "git+https://git.gnome.org/browse/gnome-shell-sass")
sha256sums=('SKIP'
            'SKIP'
            'SKIP')

pkgver() {
  cd $pkgname
  git describe --tags | sed 's/-/+/g'
}

prepare() {
  mkdir build
  cd $pkgname

  # Move the plugin to our custom epiphany-only dir
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
