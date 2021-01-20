# Originally copied from [AUR](https://aur.archlinux.org/cgit/aur.git/tree/PKGBUILD?h=st).

# Maintainer: Jose Riha <jose1711 gmail com>
# Contributor: Patrick Jackson <PatrickSJackson gmail com>
# Contributor: Christoph Vigano <mail@cvigano.de>

pkgname=st
pkgver=$(grep -m 1 --color=never "^VERSION = " config.mk | tr -d "VERSION = ")
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X.'
arch=('i686' 'x86_64' 'armv7h')
license=('MIT')
depends=('libxft' 'libxext' 'xorg-fonts-misc')
makedepends=('ncurses')
url="http://st.suckless.org"

prepare() {
  # Merge remote patch branches.
  git branch --list -r "*/patch-*" | while read branch; do
    echo "Merging branch: $branch"
    git merge $branch -m "$branch"
  done

  # Create a directory in srcdir and cd into it.
  mkdir -p $srcdir/$pkgname-$pkgver
  cd $srcdir/$pkgname-$pkgver

  # Copy all files at the root of the repo into the new directory and ignore errors.
  cp $startdir/* . 2> /dev/null || true

  # Remove unneeded files.
  rm -f PKGBUILD README.md $pkgname-*.tar.zst
}

build() {
  cd $srcdir/$pkgname-$pkgver
  make X11INC=/usr/include/X11 X11LIB=/usr/lib/X11
}

package() {
  cd $srcdir/$pkgname-$pkgver
  make PREFIX=/usr DESTDIR="$pkgdir" TERMINFO="$pkgdir/usr/share/terminfo" install
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -Dm644 README "$pkgdir/usr/share/doc/$pkgname/README"
  # remove to avoid conflict with ncurses
  rm "${pkgdir}/usr/share/terminfo/s/st" "${pkgdir}/usr/share/terminfo/s/st-256color"
}
