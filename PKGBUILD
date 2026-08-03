# Maintainer: snuglinux

pkgname=mkinitcpio-autohooks
pkgver=0.0.4
pkgrel=1
pkgdesc="Auto-manage mkinitcpio HOOKS based on mdraid/LUKS presence and rebuild initramfs"
arch=('any')
license=('GPL2')
depends=('bash' 'coreutils' 'gawk' 'grep' 'mkinitcpio' 'util-linux')
source=("https://github.com/snuglinux/$pkgname/archive/refs/tags/$pkgver.tar.gz")
sha256sums=('051aad76568aedce67b827f26c2d01d58ae78ff3302085bd3a1e492794523219')

package() {
  cd "$srcdir/${pkgname}-${pkgver}"
  install -Dm755 mkinitcpio-autohooks         "${pkgdir}/usr/bin/mkinitcpio-autohooks"
  install -Dm644 90-mkinitcpio-autohooks.hook "${pkgdir}/usr/share/libalpm/hooks/90-mkinitcpio-autohooks.hook"
  install -Dm644 autohooks.conf               "${pkgdir}/etc/mkinitcpio.conf.d/autohooks.conf"
  install -Dm644 README.md                    "${pkgdir}/usr/share/doc/${pkgname}/README"
  install -Dm644 LICENSE                      "${pkgdir}/usr/share/licenses/$pkgname/LICENSE"
}
