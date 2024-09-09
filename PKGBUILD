# Maintainer: Bernhard Landauer <bernhard[at]manjaro[dot]org>
# Maintainer: Philip Müller <philm[at]manjaro[dot]org>
# Contributor: Gerd Röthig (DAC24)
# Contributor: Thomas Baechler <thomas@archlinux.org>
# Contributor: Alonso Rodriguez <alonsorodi20 (at) gmail (dot) com>
# Contributor: Sven-Hendrik Haase <sh@lutzhaase.com>
# Contributor: Felix Yan <felixonmars@archlinux.org>
# Contributor: loqs
# Contributor: Dede Dindin Qudsy <xtrymind+gmail+com>
# Contributor: Ike Devolder <ike.devolder+gmail+com>

_linuxprefix=linux61-rt
_extramodules=extramodules-6.1-rt-MANJARO

pkgname=$_linuxprefix-nvidia-390xx
pkgdesc="NVIDIA drivers for linux"
pkgver=390.157
pkgrel=29
arch=('x86_64')
url="http://www.nvidia.com/"
license=('custom')
groups=("$_linuxprefix-extramodules")
depends=("$_linuxprefix" "nvidia-utils=$pkgver")
makedepends=("$_linuxprefix-headers")
provides=("nvidia=$pkgver" 'NVIDIA-MODULE')
replaces=('linux515-rt-nvidia-390xx' 'linux60-rt-nvidia-390xx')
options=(!strip)
install=nvidia.install
_durl="https://us.download.nvidia.com/XFree86/Linux-x86"
source=("${_durl}_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run"
        'gcc14.patch')
sha256sums=('162317a49aa5a521eb888ec12119bfe5a45cec4e8653efc575a2d04fb05bf581'
            'af840e7e03aa9cf311c0d1e32469596e5e728a0206cbe06f99bbc22e3de25a12')

_pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
    sh "${_pkg}.run" --extract-only

    cd "${_pkg}"
    patch -Np1 -i ${srcdir}/gcc14.patch
}

build() {
    _kernver="$(cat /usr/lib/modules/${_extramodules}/version)"

    cd "${_pkg}"
    export IGNORE_PREEMPT_RT_PRESENCE=1
    make -C kernel SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
    cd "${_pkg}"
    install -Dm644 kernel/*.ko -t "${pkgdir}/usr/lib/modules/${_extramodules}/"

    # compress each module individually
    find "${pkgdir}" -name '*.ko' -exec xz -T1 {} +

    install -Dm644 LICENSE -t "${pkgdir}/usr/share/licenses/${pkgname}/"
}
