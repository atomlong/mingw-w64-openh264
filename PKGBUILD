# Maintainer: drakkan <nicola.murino at gmail dot com>
pkgname=mingw-w64-openh264
pkgver=2.0.0
pkgrel=5
pkgdesc="OpenH264 is a codec library which supports H.264 encoding and decoding (mingw-w64)"
arch=(any)
url="http://www.openh264.org/"
license=("BSD")
depends=('mingw-w64-gcc')
makedepends=('nasm' 'mingw-w64-environment')
options=(!strip !buildflags staticlibs)
source=("https://github.com/cisco/openh264/archive/v${pkgver}.tar.gz")
sha256sums=('73c35f80cc487560d11ecabb6d31ad828bd2f59d412f9cd726cc26bfaf4561fd')

_architectures="i686-w64-mingw32 x86_64-w64-mingw32"


build() {
  for _arch in ${_architectures}; do
    unset CPPFLAGS
    unset CFLAGS
    unset CXXFLAGS
    unset LDFLAGS
    source mingw-env ${_arch}
    [[ -d "build-${_arch}" ]] && rm -rf "build-${_arch}"
    cp -rf "$srcdir/openh264-${pkgver}" "${srcdir}/build-${_arch}"

    pushd build-${_arch}	
    if [ ${_arch} = "i686-w64-mingw32" ]; then
      _targetarch="i686"
    else
      _targetarch="x86_64"
    fi
    make OS=mingw_nt ARCH=${_targetarch} CC=${CC} CXX=${CXX} AR=${AR}
    popd	
  done
}

package() {
  for _arch in ${_architectures}; do
    unset CPPFLAGS
    unset CFLAGS
    unset CXXFLAGS
    unset LDFLAGS
    source mingw-env ${_arch}
    cd "${srcdir}/build-${_arch}"

    if [ ${_arch} = "i686-w64-mingw32" ]; then
      _targetarch="i686"
    else
      _targetarch="x86_64"
    fi

    make OS=mingw_nt ARCH=${_targetarch} CC=${CC} CXX=${CXX} AR=${AR} DESTDIR="${pkgdir}" PREFIX="/usr/${_arch}" install
 
    install -Dm755 h264dec.exe "$pkgdir"/usr/${_arch}/bin/h264dec.exe
    install -Dm755 h264enc.exe "$pkgdir"/usr/${_arch}/bin/h264enc.exe

    ${_arch}-strip --strip-unneeded "$pkgdir"/usr/${_arch}/bin/*.dll
    ${_arch}-strip -g "$pkgdir"/usr/${_arch}/lib/*.a
    ${_arch}-strip --strip-all "$pkgdir"/usr/${_arch}/bin/*.exe
  done
}

# vim: ts=2 sw=2 et:
