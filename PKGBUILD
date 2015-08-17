# Syco <SycoLTH at gmail dot com>

pkgname=nvidia-ck-256
pkgver=256.53
_kernver='2.6.36-ck'
pkgrel=3
pkgdesc="NVIDIA drivers for kernel26-ck."
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('kernel26-ck>=2.6.36' 'kernel26-ck<2.6.37' "nvidia-utils=${pkgver}")
makedepends=('kernel26-ck-headers>=2.6.36' 'kernel26-ck-headers<2.6.37')
conflicts=('nvidia-96xx-all' 'nvidia-173xx-all' 'nvidia-ck-stable' 'nvidia-beta-ck')
provides=("nvidia=${pkgver}")
license=('custom')
install=nvidia.install

if [ "$CARCH" = "i686" ]; then
    _arch='x86'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run" "patch_2.6.36.patch")
    md5sums=('2ec917f51e752802646920e9bcd747e7' '291a1b8b954d5b2129102dc23d89c139')
elif [ "$CARCH" = "x86_64" ]; then
    _arch='x86_64'
    _pkg="NVIDIA-Linux-${_arch}-${pkgver}-no-compat32"
    source=("ftp://download.nvidia.com/XFree86/Linux-${_arch}/${pkgver}/${_pkg}.run" "patch_2.6.36.patch")
    md5sums=('73f08a19e00d05165cbbfc74e2fa4bdd' '291a1b8b954d5b2129102dc23d89c139')
fi

build() {
    cd $srcdir
    sh ${_pkg}.run --extract-only
    cd ${_pkg}
    patch -p0 < $srcdir/patch_2.6.36.patch
    cd kernel
    make SYSSRC=/lib/modules/${_kernver}/build module
}

package() {
    install -D -m644 $srcdir/${_pkg}/kernel/nvidia.ko \
        $pkgdir/lib/modules/${_kernver}/kernel/drivers/video/nvidia.ko
        install -d -m755 $pkgdir/etc/modprobe.d
        echo "blacklist nouveau" >> $pkgdir/etc/modprobe.d/nouveau_blacklist.conf
    sed -i -e "s/KERNEL_VERSION='.*'/KERNEL_VERSION='${_kernver}'/" $startdir/nvidia.install
}
