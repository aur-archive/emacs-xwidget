# Maintainer: Stefan Husmann <stefan-husmann@t-online.de>
# based on a PKGBUILD of Mathias Nedrebø <mathias <at> nedrebo.org>

pkgname=emacs-xwidget
pkgver=100835
pkgrel=1
arch=('i686' 'x86_64')
pkgdesc="Emacs, bazaar-version from the xwidget branch"
url="http://www.gnu.org/software/emacs/emacs.html"
license=('GPL3')
depends=('gconf' 'alsa-lib' 'librsvg' 'gpm' 'giflib' 'desktop-file-utils' 'hicolor-icon-theme' 'libwebkit3' 'imagemagick' 'm17n-lib')
makedepends=('bzr' 'texinfo' 'glproto')
options=('docs' '!emptydirs')
provides=('emacs')
conflicts=('emacs')
install=emacs.install
backup=('usr/share/applications/emacs.desktop' 'usr/share/emacs/site-lisp/subdirs.el')
source=()
md5sums=()
_bzrtrunk=http://bzr.savannah.gnu.org/r/emacs/xwidget
_bzrmod=emacs-xwidget

build() {
  if [[ -d $_bzrmod/.bzr ]]; then
    (cd $_bzrmod && bzr update -v && cd ..)
    msg "Local checkout updated or server timeout"
  else
    bzr co --lightweight -v $_bzrtrunk $_bzrmod
    msg "Checkout done or server timeout"
  fi
  
  msg "checkout done"
  msg "starting build ..."

  [ -d $srcdir/${_bzrmod}-build ] && rm -rf $srcdir/${_bzrmod}-build
  cp -r $srcdir/${_bzrmod} $srcdir/${_bzrmod}-build

  # Build
  cd $srcdir/${_bzrmod}-build 
  export LANG=C
  export CFLAGS="`pkg-config --cflags webkitgtk-3.0 ` -DHAVE_WEBKIT_OSR -g"
  export LDFLAGS=`pkg-config --libs webkitgtk-3.0 `
  ./autogen.sh && ./configure --prefix=/usr --with-x-toolkit=gtk3 \
    --with-sound --libexecdir=/usr/lib --localstatedir=/var \
    --with-xwidgets 
    
  make bootstrap
  make
}

package() {
  cd $srcdir/${_bzrmod}-build 
  make DESTDIR=$pkgdir install
  chown -R root:root $pkgdir/usr
  for _i in 16x16 24x24 32x32 48x48 128x128
  do
    mv $pkgdir/usr/share/icons/hicolor/${_i}/apps/emacs.png \
      $pkgdir/usr/share/icons/hicolor/${_i}/apps/$pkgname.png
  done
  mv $pkgdir/usr/share/icons/hicolor/scalable/apps/emacs.svg \
    $pkgdir/usr/share/icons/hicolor/scalable/apps/$pkgname.svg
  mv $pkgdir/usr/share/icons/hicolor/scalable/mimetypes/emacs-document.svg \
    $pkgdir/usr/share/icons/hicolor/scalable/mimetypes/$pkgname-document.svg
  chmod 775 $pkgdir/var/games
  # adding the READMEs. 
  install -Dm644 $srcdir/${_bzrmod}-build/README \
    $pkgdir/usr/share/doc/$pkgname/README
  install -Dm644 $srcdir/${_bzrmod}-build/README.xwidget \
    $pkgdir/usr/share/doc/$pkgname/README.xwidget
}
