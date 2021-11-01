# Maintainer: Sebastian Ehlert <awvwgk@disroot.org>

_realname=xtb
pkgbase=mingw-w64-${_realname}
pkgname="${MINGW_PACKAGE_PREFIX}-${_realname}"
pkgver=6.4.1
pkgrel=1
arch=('any')
mingw_arch=('mingw32' 'mingw64' 'ucrt64')
pkgdesc="Semiempirical extended tight-binding program package"
depends=("${MINGW_PACKAGE_PREFIX}-gcc-libs"
         "${MINGW_PACKAGE_PREFIX}-gcc-libgfortran"
         "${MINGW_PACKAGE_PREFIX}-openblas")
makedepends=("${MINGW_PACKAGE_PREFIX}-gcc-fortran"
             "${MINGW_PACKAGE_PREFIX}-cmake"
             "${MINGW_PACKAGE_PREFIX}-ninja")
options=('strip')
license=('LGPL-3.0-or-later')
source=(${_realname}-${pkgver}.tar.gz::"https://github.com/grimme-lab/xtb/releases/download/v${pkgver}/xtb-${pkgver}.tar.xz")
sha256sums=('15cfd8743dd963c54e0fee12da5ee34a08dde78625cf8a0a21eee1eaa652beae')

build() {
  cd "${srcdir}/${_realname}-${pkgver}"
  local _build="build_${CARCH}"
  local _fc="${MINGW_PREFIX}/bin/gfortran"
  local _cc="${MINGW_PREFIX}/bin/gcc"
  local _cmake="${MINGW_PREFIX}/bin/cmake"
  local _stack=16777216

  FC="${_fc}" CC="${_CC}" \
  MSYS2_ARG_CONV_EXCL="-DCMAKE_INSTALL_PREFIX=" \
  "${_cmake}" \
    -B"${_build}" \
    -GNinja \
    -DCMAKE_INSTALL_PREFIX="${MINGW_PREFIX}" \
    -DCMAKE_INSTALL_LIBDIR=lib \
    -DCMAKE_BUILD_TYPE=Release \
    -DBLA_VENDOR=OpenBLAS \
    -DCMAKE_EXE_LINKER_FLAGS="-Wl,--stack=${_stack}"

  "${_cmake}" \
     --build "${_build}"
}

check () {
  cd "${srcdir}/${_realname}-${pkgver}"
  local _build="build_${CARCH}"
  local _ctest="${MINGW_PREFIX}/bin/ctest"

  "${_ctest}" \
    --test-dir "${_build}" \
    --output-on-failure
}

package() {
  cd "${srcdir}/${_realname}-${pkgver}"
  local _build="build_${CARCH}"
  local _cmake="${MINGW_PREFIX}/bin/cmake"

  DESTDIR="${pkgdir}" \
  "${_cmake}" \
    --install "${_build}"
}
