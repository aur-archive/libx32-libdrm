# $Id: PKGBUILD 75476 2012-08-25 23:25:08Z lcarlier $
# Maintainer: Laurent Carlier <lordheavym@gmail.com>
# Contributor: Jan de Groot <jgc@archlinux.org>

_pkgbasename=libdrm
pkgname=libx32-$_pkgbasename
pkgver=2.4.39
pkgrel=1.1
pkgdesc="Userspace interface to kernel DRM services (x32 ABI)"
arch=(x86_64)
license=('custom')
depends=('libx32-libpciaccess' $_pkgbasename)
makedepends=(gcc-multilib)
options=('!libtool')
url="http://dri.freedesktop.org/"
source=(http://dri.freedesktop.org/${_pkgbasename}/${_pkgbasename}-${pkgver}.tar.bz2
        no-pthread-stubs.patch)
sha256sums=('386b17388980504bca16ede81ceed4c77b12c3488f46ecb7f4d48e48512a733d'
            '66fb39be073c634abc7c2af238535a63b2a03990888eb8cc5ea79fa3ef083930')

build() {
  cd "${srcdir}/${_pkgbasename}-${pkgver}"

  export CC="gcc -mx32"
  export CXX="g++ -mx32"
  export PKG_CONFIG_PATH="/usr/libx32/pkgconfig"

  patch -Np1 -i "${srcdir}/no-pthread-stubs.patch"

  # git fixes - currently none
  # patch -Np1 -i ${srcdir}/git_fixes.diff

  # libtoolize --force
  autoreconf --force --install
  ./configure --prefix=/usr --libdir=/usr/libx32 \
      --enable-udev \
      --enable-vmwgfx-experimental-api
  make
}

package() {
  cd "${srcdir}/${_pkgbasename}-${pkgver}"

  make DESTDIR="${pkgdir}" install

  rm -rf "${pkgdir}"/usr/{include,share,bin}
  mkdir -p "$pkgdir/usr/share/licenses"
  ln -s $_pkgbasename "$pkgdir/usr/share/licenses/$pkgname"
}
