# Maintainer: Noah Vogt <noah@noahvogt.com>
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Maintainer: Fabian Bornschein <fabiscafe@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

_base_name=gnome-shell
pkgbase=$_base_name-no-screenshot-notification
pkgname=(
  $pkgbase
  $pkgbase-docs
)
pkgver=48.0
pkgrel=1
epoch=1
pkgdesc="Next generation desktop shell - without the screenshot notification"
url="https://gitlab.gnome.org/GNOME/gnome-shell"
arch=(x86_64)
license=(GPL-3.0-or-later)
depends=(
  accountsservice
  at-spi2-core
  bash
  cairo
  dconf
  gcc-libs
  gcr-4
  gdk-pixbuf2
  gjs
  glib2
  glibc
  gnome-autoar
  gnome-desktop-4
  gnome-session
  gnome-settings-daemon
  graphene
  gsettings-desktop-schemas
  gtk4
  hicolor-icon-theme
  json-glib
  libadwaita
  libcanberra-pulse
  libgdm
  libgirepository
  libglvnd
  libgweather-4
  libibus
  libical
  libnm
  libnma-gtk4
  libpipewire
  libpulse
  libsecret
  libsoup3
  libx11
  libxfixes
  mutter
  pango
  polkit
  systemd-libs
  unzip
  upower
  webkitgtk-6.0
)
makedepends=(
  asciidoc
  bash-completion
  evolution-data-server
  gi-docgen
  git
  glib2-devel
  gnome-keybindings
  gobject-introspection
  meson
  python-docutils
  sassc
)
conflicts=(gnome-shell)
provides=(gnome-shell)
# GNOME Shell tags use SSH signatures which makepkg doesn't understand
source=(
  "git+https://gitlab.gnome.org/GNOME/gnome-shell.git#tag=${pkgver/[a-z]/.&}"
  "git+https://gitlab.gnome.org/GNOME/libgnome-volume-control.git#commit=5f9768a2eac29c1ed56f1fbb449a77a3523683b6"
  "git+https://github.com/ptomato/jasmine-gjs.git#commit=856465dddbd92e82e574891e1ebc79e17d7b708a"
  "disable-screenshot-notification.patch"
  "disable-screenshot-sound.patch"
  "change-screenshot-filenaming.patch"
)
b2sums=('8c399191765672c682a4288a8a6450871938588870fa177ffbadf26369221af49a85ec3e45ad1ffd00d3d4ee4b93e4076bb16ff40b7fcd98531f8365d3f9ee2b'
        'e31ae379039dfc345e8032f7b9803a59ded075fc52457ba1553276d3031e7025d9304a7f2167a01be2d54c5e121bae00a2824a9c5ccbf926865d0b24520bb053'
        'ecbbb9ce5895cc1caed2ddef39c70b4768d78ea0a929ea932d4149f923f92650973cdaefc2aacc9063f2ccf4ec965b57a9698a286f9a6561e39ce2e579ae4522'
        '50566ed521cb7fee15ad0ab25733848253ffc01827560181283eb2dafc3d6e9b6a917f1640ff3912e05e5fd6ff66e679e37e793cbe94927daa6e055198ac4de2'
        '4a70c4a8ca1aded2676873d9cfa15845c008b0747aa32646cd7e4f90d65dc13b1f58555f11957a59fe2e51a11c3409317fc8a2c290ebde0a4da380b11accbf36'
        '18c6930cf015ed55b635edebff9e685b5ddc0a20225e4e7e7289f3eaba9b626b1c036a16d4af68a676e6ac2e42b68cced7240229ba170c3e851a87543bf62435')

prepare() {
  # Inject gvc
  ln -s libgnome-volume-control gvc

  cd $_base_name

  patch -p1 -i "$srcdir/disable-screenshot-notification.patch"
  patch -p1 -i "$srcdir/disable-screenshot-sound.patch"
  patch -p1 -i "$srcdir/change-screenshot-filenaming.patch"
}

build() {
  local meson_options=(
    -D gtk_doc=true
    -D tests=false
  )

  CFLAGS="${CFLAGS/-O2/-O3} -fno-semantic-interposition"
  LDFLAGS+=" -Wl,-Bsymbolic-functions"

  # Inject gvc
  export MESON_PACKAGE_CACHE_DIR="$srcdir"

  arch-meson $_base_name build "${meson_options[@]}"
  meson compile -C build
}


package_gnome-shell-no-screenshot-notification() {
  depends+=(libmutter-16.so)
  conflicts=($_base_name)
  provides=($_base_name)
  optdepends=(
    'evolution-data-server: Evolution calendar integration'
    'gnome-bluetooth-3.0: Bluetooth support'
    'gnome-control-center: System settings'
    'gnome-disk-utility: Mount with keyfiles'
    'gst-plugin-pipewire: Screen recording'
    'gst-plugins-good: Screen recording'
    'power-profiles-daemon: Power profile switching'
    'python-gobject: gnome-shell-test-tool performance tester'
    'python-simplejson: gnome-shell-test-tool performance tester'
    'switcheroo-control: Multi-GPU support'
  )
  groups=(gnome)

  meson install -C build --destdir "$pkgdir"

  mkdir -p doc/usr/share
  mv {"$pkgdir",doc}/usr/share/doc
}

package_gnome-shell-no-screenshot-notification-docs() {
  pkgdesc+=" (API documentation)"
  depends=()
  conflicts=($_base_name-docs)
  provides=($_base_name-docs)

  mv doc/* "$pkgdir"
}

# vim:set sw=2 sts=-1 et:
