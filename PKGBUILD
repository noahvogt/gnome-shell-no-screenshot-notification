# Maintainer: Noah Vogt <noah@noahvogt.com>
# Maintainer: Jan Alexander Steffens (heftig) <heftig@archlinux.org>
# Contributor: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

_base_name=gnome-shell
pkgname=gnome-shell-no-screenshot-notification
pkgver=45.1
pkgrel=2
epoch=1
pkgdesc="Next generation desktop shell - without the screenshot notification"
url="https://wiki.gnome.org/Projects/GnomeShell"
arch=(x86_64)
license=(GPL)
depends=(
  accountsservice
  gcr-4
  gjs
  gnome-autoar
  gnome-session
  gnome-settings-daemon
  gsettings-desktop-schemas
  gtk4
  libadwaita
  libcanberra-pulse
  libgdm
  libgweather-4
  libibus
  libnma-gtk4
  libsecret
  libsoup3
  mutter
  unzip
  upower
)
makedepends=(
  asciidoc
  bash-completion
  evolution-data-server
  git
  gnome-control-center
  gobject-introspection
  gtk-doc
  meson
  sassc
)
checkdepends=(
  appstream-glib
  python-dbusmock
  xorg-server-xvfb
)
optdepends=(
  'evolution-data-server: Evolution calendar integration'
  'gnome-bluetooth-3.0: Bluetooth support'
  'gnome-control-center: System settings'
  'gnome-disk-utility: Mount with keyfiles'
  'gst-plugin-pipewire: Screen recording'
  'gst-plugins-good: Screen recording'
  'power-profiles-daemon: Power profile switching'
  'python-gobject: gnome-shell-test-tool performance tester'
  'switcheroo-control: Multi-GPU support'
)
conflicts=(gnome-shell)
provides=(gnome-shell)
groups=(gnome)
_commit=a7c169c6c29cf02a4c392fa0acbbc5f5072823e7  # tags/45.1^0
source=(
  "git+https://gitlab.gnome.org/GNOME/gnome-shell.git#commit=$_commit"
  "git+https://gitlab.gnome.org/GNOME/libgnome-volume-control.git"
  "disable-screenshot-notification.patch"
  "disable-screenshot-sound.patch"
  "change-screenshot-filenaming.patch"
)
b2sums=('SKIP'
        'SKIP'
        'a726caac6ec6dea7f7c8d122cedf30ad0a44921321ed763e2b4422e6f455edf2a93f6d9de72ac8c10560f69bb987a37acf6de2580b36464345afc08ab439d86e'
        'aca63da0703e44401fa987660d9542d92df7e89da66d93dbf0cd75bc25868b75d9502fb73c6981d964ebbb3b6a719c9b1d84629560dafbfedf3781a6fab7769a'
        'e3d93ff6565a4e2db845fe95bfd965d39a042beb47a89a91aea5f7c21b7c4d52d377470b9803082d33924780cb5a82b06159cd6ace14e1ae4249ba08eda71dac')

pkgver() {
  cd $_base_name
  git describe --tags | sed 's/[^-]*-g/r&/;s/-/+/g'
}

prepare() {
  cd $_base_name

  patch -p1 -i "$srcdir/disable-screenshot-notification.patch"
  patch -p1 -i "$srcdir/disable-screenshot-sound.patch"
  patch -p1 -i "$srcdir/change-screenshot-filenaming.patch"

  git submodule init
  git submodule set-url subprojects/gvc "$srcdir/libgnome-volume-control"
  git -c protocol.file.allow=always submodule update
}

build() {
  local meson_options=(
    -D gtk_doc=true
  )

  CFLAGS="${CFLAGS/-O2/-O3} -fno-semantic-interposition"
  LDFLAGS+=" -Wl,-Bsymbolic-functions"

  arch-meson $_base_name build "${meson_options[@]}"
  meson compile -C build
}

_check() (
  export XDG_RUNTIME_DIR="$PWD/rdir"
  mkdir -p -m 700 "$XDG_RUNTIME_DIR"

  export NO_AT_BRIDGE=1 GTK_A11Y=none

  # meson test -C build --print-errorlogs -t 3
)

check() {
  dbus-run-session xvfb-run -s '-nolisten local +iglx -noreset' \
    bash -c "$(declare -f _check); _check"
}

package() {
  depends+=(libmutter-13.so)
  meson install -C build --destdir "$pkgdir"
}

# vim:set sw=2 sts=-1 et:
