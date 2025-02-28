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
pkgver=47.4
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
  "disable-screenshot-notification.patch"
  "disable-screenshot-sound.patch"
  "change-screenshot-filenaming.patch"
)
b2sums=('d8e8653f0e2a645390b95e16b45efd80501ce829ada0671b7c1cd597575ea64d2f650ee4771cbe60d41175bc7823eabe8dde1f58ba3c6b2cee6166b6f5c1f45a'
        'e31ae379039dfc345e8032f7b9803a59ded075fc52457ba1553276d3031e7025d9304a7f2167a01be2d54c5e121bae00a2824a9c5ccbf926865d0b24520bb053'
        'f05b5daf8194566769ac5123b29e06caaf93add8d398c4545a4690e9558dafd85fabbe27301aaedf9c69d45499907c7b2d9d8ae16e4c989a6cfb846c91c3adf6'
        '26702aa16b023c049521d7af391e21533bdbf065ad3c316949dade33651347cca7998e9e4fabe5fa913426cd45ad5720b0f80c5ad10f98ed517d46aeea497332'
        '06b1ae01d08bfaf3c2d3a55e00985ae76120204afa002f65465c61bf0d034046ed926ae10a7262c0838cabf8d5f26eb469fbdfe610469f831678c31de8a64a20')

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
  depends+=(libmutter-15.so)
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
