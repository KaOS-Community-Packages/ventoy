pkgname=ventoy
pkgver=1.0.97
pkgrel=1
pkgdesc="A new universal bootable USB solution"
arch=('x86_64')
url="http://www.ventoy.net"
license=('GPL-3.0-or-later')
depends=('bash' 'util-linux' 'xz' 'dosfstools' 'qt5-base')
optdepends=('gtk3: GTK3 GUI'
            'polkit: Launch GUI from application menu')
install="${pkgname}.install"
source=("${pkgname}-${pkgver}.tar.gz::https://github.com/ventoy/Ventoy/releases/download/v${pkgver}/${pkgname}-${pkgver}-linux.tar.gz"
        "${pkgname}"
        "${pkgname}-2-disk"
        "${pkgname}-web"
        "${pkgname}-plugson"
        "${pkgname}-persistent"
        "${pkgname}-extend-persistent"
        "${pkgname}.desktop"
        'sanitize.patch')
sha256sums=('SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP' 'SKIP')

prepare() {
  cd "${srcdir}/${pkgname}-${pkgver}/"

  # Decompress tools
  pushd "tool/${CARCH}"
  for file in *.xz; do
    xzcat "${file}" > "${file%.xz}"
    chmod +x "${file%.xz}"
  done

  # Cleanup .xz crap
  rm -fv ./*.xz
  popd

  msg2 "Applying sanitize patch"
  patch --verbose -p0 < "${srcdir}/sanitize.patch"

  # Log location
  sed -i 's|log\.txt|/var/log/ventoy.log|g' WebUI/static/js/languages.js tool/languages.json

  # Non-POSIX compliant scripts
  sed -i 's|bin/sh|usr/bin/env bash|g' tool/{ventoy_lib.sh,VentoyWorker.sh}

  msg2 "Cleaning up unused binaries"
  # Preserving mkexfatfs and mount.exfat-fuse because exfatprogs is incompatible
  for binary in xzcat hexdump; do
    rm -fv tool/${CARCH}/${binary}
  done
}

package() {
  cd "${srcdir}/${pkgname}-${pkgver}/"

  install -dm755 "${pkgdir}/opt/Ventoy/"
  cp -a "${srcdir}/${pkgname}-${pkgver}/." "${pkgdir}/opt/Ventoy/"
  install -d "${pkgdir}/usr/share/pixmaps/"
  install -Dm644 "${srcdir}/${pkgname}-${pkgver}/WebUI/static/img/VentoyLogo.png" "${pkgdir}/usr/share/pixmaps/${pkgname}.png"
  install -d "${pkgdir}/usr/share/applications"
  install -Dm644 "${srcdir}/${pkgname}.desktop" -t "${pkgdir}/usr/share/applications"

  msg2 "Linking system binaries"
  for binary in xzcat hexdump; do
    ln -svf "/usr/bin/${binary}" "${pkgdir}/opt/Ventoy/tool/${CARCH}/"
  done

  install -dm755 "${pkgdir}/usr/bin/"
  install -Dm755 "${srcdir}/${pkgname}"{,-2-disk,-web,-plugson,-{,extend-}persistent} -t "${pkgdir}/usr/bin/"

  msg2 "Removing GTK 2 files"
  if [ ${CARCH} == "x86_64" ]; then
    rm -fv "${pkgdir}/opt/Ventoy/tool/${CARCH}/Ventoy2Disk.gtk2"
  fi

  msg2 "Removing unnecessary tool architectures"
  rm -fdrv "${pkgdir}"/opt/Ventoy/VentoyGUI.{aarch64,i386,mips64el}
  rm -fdrv "${pkgdir}"/opt/Ventoy/tool/{aarch64,i386,mips64el}
}
