# $Id: PKGBUILD 299071 2017-06-20 16:53:25Z juergen $
# Maintainer: Jörg Schuck <joerg_schuck@web.de>
# Contributor: Juergen Hoetzel <juergen@archlinux.org>
# Contributor: Mark Schneider <queueRAM@gmail.com>

pkgname=gnucash
pkgver=3.0
pkgrel=1
pkgdesc="A personal and small-business financial-accounting application"
arch=('i686' 'x86_64')
url="http://www.gnucash.org"
license=("GPL")
depends=('slib' 'libdbi-drivers' 'libmariadbclient' 'postgresql-libs' 'aqbanking' 'webkit2gtk' 'libgnome-keyring' 'libgnomecanvas' 'boost-libs')
makedepends=('intltool' 'boost' 'swig' 'gtest' 'gmock' 'gconf' 'cmake')
optdepends=('evince: for print preview'
	    'yelp: help browser'
            'perl-finance-quote: for stock information lookups'
            'perl-date-manip: for stock information lookups')
options=('!makeflags' '!emptydirs')
source=(
	http://downloads.sourceforge.net/sourceforge/${pkgname}/${pkgname}-${pkgver}.tar.bz2
)
sha1sums=('a575e853668b93b34dcd94f0ef0d1fee25b0165f')
sha256sums=('4c754476a5b80a97abacaeadac64fefc5a68fcfec15967908dbe3c9f7370dbb9')
sha512sums=('5ec13b8abe1520a7e614ceeca4c41d5dba3ebae4ec965918584963022ceb5cb3b85862289a85a72767db74a0c735214a78342a53c37e6da939ff850538174a87')
backup=(
	'etc/gnucash/config'
	'etc/gnucash/environment'
)

prepare() {
  cd "${srcdir}"

  mkdir build
  cd build
  cmake -D CMAKE_INSTALL_PREFIX=/usr "${srcdir}/${pkgname}-${pkgver}"

}

build() {
  cd "${srcdir}/build"

  make -j $(cat /proc/cpuinfo | grep -ci '^processor')
}

package() {
  cd "${srcdir}/build"
  make GCONF_DISABLE_MAKEFILE_SCHEMA_INSTALL=1 DESTDIR="${pkgdir}" install

  install -dm755 "${pkgdir}/usr/share/gconf/schemas"
  gconf-merge-schema "${pkgdir}/usr/share/gconf/schemas/${pkgname}.schemas" --domain gnucash "${pkgdir}"/etc/gconf/schemas/*.schemas
  rm -f "${pkgdir}"/etc/gconf/schemas/*.schemas

  # Delete the gnucash-valgrind executable because the source files
  # are not included with the package and the executable is hardlinked
  # to the location that it was built at.
  rm -f "${pkgdir}"/usr/bin/gnucash-valgrind

}
