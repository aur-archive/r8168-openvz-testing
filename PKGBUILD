# Maintainer:   Lucky <archlinux@builds.lucky.li>
# Based on r8168 [community]

pkgname=r8168-openvz-testing
_realname=r8168
pkgver=8.039.00
pkgrel=4
pkgdesc="A kernel module for Realtek 8168 network cards for linux-openvz-testing"
url="http://www.realtek.com.tw"
license=("GPL")
arch=("i686" "x86_64")
depends=("glibc" "linux-openvz-testing")
makedepends=("linux-openvz-testing-headers")
source=("https://r8168dl.appspot.com/files/${_realname}-${pkgver}.tar.bz2")
install="${_realname}.install"
md5sums=("98314a558a4c667d7652d5dbdd0f0579")
sha256sums=("767d922270274e781d8d42493a0021db1cafcb0388ac62564d0c0c3d82703edd")

build() {
  _kernver=$(pacman -Q linux-openvz-testing | sed -r 's#.* ([0-9]+\.[0-9]+\.[0-9]+).*#\1#')
  KERNEL_VERSION=$(cat /usr/lib/modules/extramodules-${_kernver}-openvz-testing/version)

  cd "${_realname}-${pkgver}"

  # avoid using the Makefile directly -- it doesn't understand
  # any kernel but the current.
  make -C "/usr/lib/modules/${KERNEL_VERSION}/build" \
      SUBDIRS="${srcdir}/${_realname}-${pkgver}/src" \
      EXTRA_CFLAGS="-DCONFIG_R8168_NAPI -DCONFIG_R8168_VLAN" \
      modules
}

package() {
  _kernver=$(pacman -Q linux-openvz-testing | sed -r 's#.* ([0-9]+\.[0-9]+\.[0-9]+).*#\1#')
  depends=("linux-openvz-testing>=${_kernver}" "linux-openvz-testing<${_kernver/${_kernver/*.}}$(expr ${_kernver/*.} + 1)")
  KERNEL_VERSION=$(cat /usr/lib/modules/extramodules-${_kernver}-openvz-testing/version)
  msg "Kernel = ${KERNEL_VERSION}"

  cd "${_realname}-${pkgver}"
  install -Dm644 "src/${_realname}.ko" "${pkgdir}/usr/lib/modules/extramodules-${_kernver}-openvz-testing/${_realname}.ko"
  find "${pkgdir}" -name '*.ko' -exec gzip -9 {} +

  sed -i "s|extramodules-.*-openvz-testing|extramodules-${_kernver}-openvz-testing|" "${startdir}/${_realname}.install"
}
