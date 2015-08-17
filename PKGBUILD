# Maintainer: kevku <kevku@gmx.com>
pkgname=sk-esteidpkcs11loader-svn
_pkgname=esteid-pkcs11-module-loader
_browser=firefox
pkgver=62
pkgrel=5
pkgdesc="Estonian ID-card Firefox extension (Official version by AS Sertifitseerimiskeskus)"
arch=('x86_64' 'i686')
url="http://www.id.ee/"
license=('LGPL')
depends=('opensc012')
makedepends=('cmake' 'subversion' 'unzip' 'zip')
conflicts=('esteid-browser-plugin-svn')

_svntrunk=https://svn.eesti.ee/projektid/idkaart_public/trunk/$_pkgname
_svnmod=$_pkgname

build() {
  cd "$srcdir"
  rm -rf "$srcdir/$_svnmod"
  if [ -d $_svnmod/.svn ]; then
    (cd $_svnmod && svn up -r $pkgver)
  else
    svn co $_svntrunk --config-dir ./ -r $pkgver $_svnmod
  fi

  msg "SVN checkout done or server timeout"
  msg "Starting make..."

  rm -rf "$srcdir/$_svnmod-build"
  cp -r "$srcdir/$_svnmod" "$srcdir/$_svnmod-build"
  cd "$srcdir/$_svnmod-build/Mozilla"

  sed -i 's|(bits == 64) ? "/usr/lib64/" :||g' chrome/content/pkcs11-loader.js
  sed -i 's|/usr/etc/digidocpp/certs/|/usr/share/esteid/certs/|g' chrome/content/pkcs11-loader.js
  #sed -i 's|onepin-opensc-pkcs11|opensc-pkcs11|g' chrome/content/pkcs11-loader.js
  cmake . -DCMAKE_INSTALL_PREFIX=/usr -DCMAKE_INSTALL_LIBDIR="lib" -DCMAKE_INSTALL_SYSCONFDIR="/etc"
  make
}

package() {
  cd "$srcdir/$_svnmod-build/Mozilla"
  mkdir -p "$pkgdir/usr/lib/$_browser/extensions"
  cp -r "{aa84ce40-4253-a00a-8cd6-0800200f9a66}" "$pkgdir/usr/lib/$_browser/extensions/"
}
