# Maintainer: Piotr Gorski <lucjan.lucjanov@gmail.com>

pkgname=nvidia-bfs-304xx
pkgver=304.128
_extramodules=extramodules-4.3-bfs
pkgrel=6
_pkgdesc="NVIDIA 304xx drivers for linux-bfs."
pkgdesc="$_pkgdesc"
arch=('i686' 'x86_64')
url="http://www.nvidia.com/"
depends=('linux-bfs>=4.3' 'linux-bfs<4.4' "nvidia-304xx-libgl" "nvidia-304xx-utils=${pkgver}")
makedepends=('linux-bfs-headers>=4.3' 'linux-bfs-headers<4.4')
conflicts=('nvidia-bfs' 'nvidia-bfs-340xx')
license=('custom')
install=nvidia-bfs-304xx.install
options=(!strip)
source=('nv-drm.patch'
        'nvidia-4.3-build.patch')
source_i686=("ftp://download.nvidia.com/XFree86/Linux-x86/${pkgver}/NVIDIA-Linux-x86-${pkgver}.run")
source_x86_64=("ftp://download.nvidia.com/XFree86/Linux-x86_64/${pkgver}/NVIDIA-Linux-x86_64-${pkgver}-no-compat32.run")
md5sums=('2365f1405f0c7bbb8f8cd7ebd5e4e301'
         'e400a8c538afd490726941d8c69b2c2d')
md5sums_i686=('be2b40a4dc3339b050a4f76ddd27e96c')
md5sums_x86_64=('6478e40ed87d9177cbfc3d0b6e39a051')


[[ "$CARCH" = "i686" ]] && _pkg="NVIDIA-Linux-x86-${pkgver}"
[[ "$CARCH" = "x86_64" ]] && _pkg="NVIDIA-Linux-x86_64-${pkgver}-no-compat32"

prepare() {
	sh "${_pkg}.run" --extract-only
	cd "${_pkg}"
	patch -p0 -i "$srcdir/nv-drm.patch"
	patch -Np1 -i "$srcdir/nvidia-4.3-build.patch"
}

build() {
	_kernver="$(cat /usr/lib/modules/${_extramodules}/version)"
	cd ${_pkg}/kernel
	make SYSSRC=/usr/lib/modules/"${_kernver}/build" module
}

package() {
	install -D -m644 "${srcdir}/${_pkg}/kernel/nvidia.ko" \
		"${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"
	install -d -m755 "${pkgdir}/usr/lib/modprobe.d"
	echo "blacklist nouveau" >> "${pkgdir}/usr/lib/modprobe.d/nvidia-bfs-304xx.conf"
	sed -i -e "s/EXTRAMODULES='.*'/EXTRAMODULES='${_extramodules}'/" "${startdir}/nvidia-bfs-304xx.install"
	gzip -9 "${pkgdir}/usr/lib/modules/${_extramodules}/nvidia.ko"

	# the license file is part of nvidia-304xx-utils - the module depends on it, so we don't ship it another time.
}