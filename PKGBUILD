# Maintainer: snuglinux

pkgname=mkinitcpio-autohooks
pkgver=0.0.1
pkgrel=1
pkgdesc="Auto-manage mkinitcpio HOOKS based on mdraid/LUKS presence and rebuild initramfs"
arch=('any')
license=('MIT')
depends=('bash' 'coreutils' 'gawk' 'grep' 'mkinitcpio' 'util-linux')
source=('mkinitcpio-autohooks'
        '90-mkinitcpio-autohooks.hook'
        'LICENSE')
sha256sums=('aec3a01445350bbfe467750384644763bac6cf1e5076563deca897d1db2c4f9e'
            'af3008bf2951780fa25eaa3890f06d54ad7bb8bc2284cbfc9940e043dfe7435f' 
            '8ceb4b9ee5adedde47b31e975c1d90c73ad27b6b165a1dcd80c7c545eb65b903')

package() {
  install -Dm755 mkinitcpio-autohooks "$pkgdir/usr/bin/mkinitcpio-autohooks"
  install -Dm644 90-mkinitcpio-autohooks.hook "$pkgdir/usr/share/libalpm/hooks/90-mkinitcpio-autohooks.hook"
  install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
