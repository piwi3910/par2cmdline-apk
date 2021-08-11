# Contributor:
# Maintainer: Pascal Watteel <pascal@watteel.com>
pkgname=par2cmdline
pkgver=0.8.1
pkgrel=1
pkgdesc="Par2cmdline is a PAR 2.0 compatible file verification and repair tool"
url="https://github.com/Parchive/par2cmdline"
arch="all"
license="GPL-2.0-only"
subpackages="$pkgname-doc"
source="$pkgname-$pkgver.tar.gz::https://github.com/Parchive/$pkgname/archive/refs/tags/v$pkgver.tar.gz"

build() {
        export LDFLAGS="-static"
        aclocal
        automake --add-missing
        autoupdate
        autoconf
        ./configure --prefix=/usr
        make
}

check() {
        make check
}

package() {
        make DESTDIR="$pkgdir" install
}

sha512sums="d8a49ae7688c9833278e7c2c666855f92f91850adc700f5e5bb2afc01c508dc50a389b822e3cf120ed6c950bb98854724c8d84b9173dd4452f8216105a42145c  par2cmdline-0.8.1.tar.gz"