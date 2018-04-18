# $Id: PKGBUILD 299071 2017-06-20 16:53:25Z juergen $
# Maintainer: Jörg Schuck <joerg_schuck@web.de>
# Contributor: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Mark Schneider <queueRAM@gmail.com>

pkgname=gnucash-git
_pkgname=gnucash
__pkgname=Gnucash
pkgver=3.0.r83.gcad6bb427
pkgrel=1
pkgdesc="A personal and small-business financial-accounting application"
arch=('i686' 'x86_64')
url="http://www.gnucash.org"
license=("GPL")
depends=('libmariadbclient' 'postgresql-libs' 'aqbanking' 'webkit2gtk' 'boost-libs' 'libsecret' 'libdbi-drivers' 'guile')
makedepends=('boost' 'gmock' 'gwenhywfar' 'cmake' 'swig')
optdepends=(
	'gnucash-docs: for documentation'
	'iso-codes: for translation of currency names'
	'perl-finance-quote: for stock information lookups'
	'perl-date-manip: for stock information lookups'
)
options=('!emptydirs')
source=("$pkgname::git+https://github.com/${__pkgname}/${_pkgname}.git")
sha256sums=('SKIP')
backup=(
	'etc/gnucash/environment'
)
conflicts=('gnucash')
provides=('gnucash')

pkgver() {
	cd "$srcdir/$pkgname"

	( set -o pipefail
	  git describe --long --tags 2>/dev/null | sed 's/\([^-]*-g\)/r\1/;s/-/./g' ||
	  printf "r%s.%s" "$(git rev-list --count HEAD)" "$(git rev-parse --short HEAD)"
	)
}

prepare() {
  cd "${srcdir}"

  mkdir build
  cd build
  cmake	-DCMAKE_INSTALL_PREFIX=/usr \
	-DCMAKE_INSTALL_LIBDIR=/usr/lib \
	-DCMAKE_INSTALL_LIBEXECDIR=/usr/lib \
	-DCOMPILE_GSCHEMAS=NO \
	-DWITH_OFX=ON \
	-DWITH_AQBANKING=ON \
	"${srcdir}/${pkgname}"

}

build() {
  cd "${srcdir}/build"

  make
}

package() {
  cd "${srcdir}/build"
  make DESTDIR="${pkgdir}" install
  
  # Delete the gnucash-valgrind executable because the source files
  # are not included with the package and the executable is hardlinked
  # to the location that it was built at.
  rm -f "${pkgdir}"/usr/bin/gnucash-valgrind

}
