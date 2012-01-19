# Maintainer: Ionut Biru <ibiru@archlinux.org>
# Contributor: Flamelab <panosfilip@gmail.com

pkgname=gnome-shell
pkgver=3.2.2
pkgrel=2
pkgdesc="The next generation GNOME Shell"
arch=('i686' 'x86_64')
url="http://live.gnome.org/GnomeShell"
license=('GPL2')
depends=('mutter' 'gjs' 'libcroco' 'gnome-bluetooth' 'gnome-desktop' 'gnome-menus' 'libpulse' 'folks' 'telepathy-logger' 'networkmanager' 'caribou' 'nautilus' 'telepathy-mission-control' 'unzip')
makedepends=('intltool' 'gnome-doc-utils')
optdepends=('network-manager-applet: shell integration for networkmanager')
options=('!libtool' '!emptydirs')
install=gnome-shell.install
groups=(gnome)
source=(http://ftp.gnome.org/pub/GNOME/sources/$pkgname/${pkgver%.*}/$pkgname-$pkgver.tar.xz
        revert-notificationdaemon-group-based-on-pid-and-titles.patch)
sha256sums=('68967b9d58ad0551d7d3d28a276526a15faf1fc1d27f4624eb405663910e2eb8'
            '9e0337cd25d29d7215561d6fa30612d69c89fe7c27aa563a0c0b8a5b6f6cf12a')

build() {
  cd "$srcdir/$pkgname-$pkgver"

  patch -Np1 -R -i "$srcdir/revert-notificationdaemon-group-based-on-pid-and-titles.patch"

  PYTHON=/usr/bin/python2 ./configure --prefix=/usr --sysconfdir=/etc \
      --libexecdir=/usr/lib/gnome-shell \
      --localstatedir=/var --disable-static \
      --disable-schemas-compile
  make
}

package() {
  cd "$srcdir/$pkgname-$pkgver"
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="$pkgdir" install

  install -m755 -d "$pkgdir/usr/share/gconf/schemas"
  gconf-merge-schema "$pkgdir/usr/share/gconf/schemas/$pkgname.schemas" --domain gnome-shell "$pkgdir"/etc/gconf/schemas/*.schemas
  rm -f "$pkgdir"/etc/gconf/schemas/*.schemas
}
