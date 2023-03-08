pkgname=ventoy
_pkgname=Ventoy
pkgver=1.0.89
pkgrel=1
pkgdesc="An open source tool to create bootable USB drive for ISO/WIM/IMG/VHD(x)/EFI files, it is a new multiboot USB solution"
arch=('x86_64')
url="http://www.ventoy.net"
license=('GPL3')
depends=('bash' 'util-linux' 'xz' 'dosfstools')
optdepends=('gtk3: GTK3 GUI'
            'qt5-base: Qt5 GUI'
            'exfatprogs: Userspace Utility for exFAT filesystem on Linux kernel'
            'polkit: run GUI from application menu'
            'polkit-qt5: QT wrapper for Polkit GUI')
provides=("${pkgname}")
conflicts=("${pkgname}")
install=("${pkgname}.install::https://github.com/KaOS-Community-Packages/${pkgname}/raw/main/ventoy.install")
source=("${pkgname}-${pkgver}-linux.tar.gz::https://github.com/${pkgname}/Ventoy/releases/download/v${pkgver}/${pkgname}-${pkgver}-linux.tar.gz"
        "${pkgname}.install::https://github.com/KaOS-Community-Packages/${pkgname}/raw/main/ventoy.install"
        "${pkgname}::https://github.com/KaOS-Community-Packages/${pkgname}/raw/main/ventoy"
        "${pkgname}gui::https://github.com/KaOS-Community-Packages/${pkgname}/raw/main/ventoygui"
        "${pkgname}web::https://github.com/KaOS-Community-Packages/${pkgname}/raw/main/ventoyweb"
        "${pkgname}plugson::https://github.com/KaOS-Community-Packages/${pkgname}/raw/main/ventoyplugson"
        "${pkgname}-persistent::https://github.com/KaOS-Community-Packages/${pkgname}/blob/main/ventoy-persistent"
        "${pkgname}-extend-persistent::https://github.com/KaOS-Community-Packages/${pkgname}/raw/main/ventoy-extend-persistent"
        "${pkgname}.desktop::https://github.com/KaOS-Community-Packages/${pkgname}/raw/main/ventoy.desktop"
        "sanitize.patch::https://raw.githubusercontent.com/KaOS-Community-Packages/${pkgname}/main/sanitize.patch")
sha256sums=('SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP'
            'SKIP')

prepare() {
  cd "${pkgname}-$pkgver"

  # Decompress tools
  pushd tool/$CARCH
  for file in *.xz; do
    xzcat $file > ${file%.xz}
    chmod +x ${file%.xz}
  done

  # Cleanup .xz crap
  rm -fv ./*.xz
  popd

  # Apply sanitize patch
  patch --verbose -p0 < "$srcdir/sanitize.patch"

  # Log location
  sed -i 's|log\.txt|/var/log/ventoy.log|g' WebUI/static/js/languages.js tool/languages.json

  # Non-POSIX compliant scripts
  sed -i 's|bin/sh|usr/bin/env bash|g' tool/{ventoy_lib.sh,VentoyWorker.sh}

  # Clean up unused binaries
  # Preserving mkexfatfs and mount.exfat-fuse because exfatprogs is incompatible
  for binary in xzcat hexdump; do
    rm -fv tool/$CARCH/$binary
  done
}

package() {
  cd "${pkgname}-$pkgver"
  install -Dm644 -vt      "$pkgdir/opt/${pkgname}/boot/"            boot/*
  install -Dm644 -vt      "$pkgdir/opt/${pkgname}/${pkgname}/"      "${pkgname}"/*
  install -Dm755 -vt      "$pkgdir/opt/${pkgname}/tool/"            tool/*.{cer,glade,json,sh,xz}
  install -Dm755 -vt      "$pkgdir/opt/${pkgname}/tool/$CARCH/"     tool/$CARCH/*
  install -Dm755 -vt      "$pkgdir/opt/${pkgname}/"                 *.sh
  cp --no-preserve=o -avt "$pkgdir/opt/${pkgname}/"                 plugin WebUI
  chmod -f 777 "$pkgdir/opt/${pkgname}/tool/$CARCH/Ventoy2Disk.qt5"

  install -Dm755 "VentoyGUI.$CARCH" -vt "$pkgdir/opt/${pkgname}"
  install -Dm644 WebUI/static/img/VentoyLogo.png -v "$pkgdir/usr/share/pixmaps/${pkgname}.png"
  install -Dm644 "$srcdir/${pkgname}.desktop" -vt "$pkgdir/usr/share/applications"

  # Link system binaries
  for binary in xzcat hexdump; do
    ln -fsv /usr/bin/$binary "$pkgdir/opt/${pkgname}/tool/$CARCH/"
  done

  install -Dm755 "$srcdir/${pkgname}"{,gui,web,plugson,-{,extend-}persistent} -vt "$pkgdir"/usr/bin/
  ln -fsv "$pkgdir/opt/$pkgname/tool/$CARCH/Ventoy2Disk.qt5" "$pkgdir/usr/bin/ventoy2disk.qt5"

  # Remove Gtk 2 files
  if [ $CARCH == "x86_64" ]; then
    rm "$pkgdir/opt/${pkgname}/tool/$CARCH/Ventoy2Disk.gtk2"
  fi
}
