# Maintainer: Ivan Gabaldon <aur[at]inetol.net>
# Contributor: sukanka <su975853527 at gmail.com>

pkgname=tradingview
_pkgname=TradingView
pkgver=2.8.1
pkgrel=1
pkgdesc='A charting platform for traders and investors'
arch=('x86_64')
url='https://www.tradingview.com/desktop/'
license=('LicenseRef-TradingView')
makedepends=('links')
_electron=electron30
source=("$pkgname-$pkgver.deb::https://tvd-packages.tradingview.com/ubuntu/stable/pool/multiverse/t/tradingview/jammy/$pkgname-$pkgver-1_amd64.deb"
        "$pkgname.sh")
b2sums=('404580b1978d7873ca3a4df7a958fb3a98e343e24573fee7aaad46b304400588a2c0238fe6ae29514e20174a3e4b3777ea5257ad431bc254e0a3810a5c498692'
        '1c7aaed8c8a4dad5030dc2f5506915e29d3b5ce19a61455db8be6821bc156ce6b779f7f4c63fd3929a141232443a4f5979e49c8ba3a18424d2854ec684e2f037')

prepare() {
    sed -i "s|@ELECTRON@|$_electron|" $pkgname.sh

    mkdir -p "$pkgname-$pkgver/"
    bsdtar -xpf 'data.tar.xz' -C "$pkgname-$pkgver/"

    # Convert
    cd "$pkgname-$pkgver/"

    links -width 80 -dump 'https://www.tradingview.com/policies/' | sed -n '/Terms of Use/,/TradingView may update these Rules at any time/p' > "opt/$_pkgname/resources/LICENSE"

    cat "../$pkgname.sh" > "opt/$_pkgname/resources/$pkgname"

    mv "usr/share/applications/$pkgname.desktop" "opt/$_pkgname/resources/$pkgname.desktop"
    sed -i "s|Exec=.*|Exec=/usr/bin/$pkgname %U|" "opt/$_pkgname/resources/$pkgname.desktop"

    mv "usr/share/icons/hicolor/512x512/apps/$pkgname.png" "opt/$_pkgname/resources/$pkgname.png"
}

package() {
    depends=("$_electron"
             'gcc-libs'
             'glib2'
             'glibc'
             'libsecret')

    optdepends=('libappindicator-gtk3: Systray indicator support')

    install -d "$pkgdir/opt/$pkgname/"
    cp -a "$pkgname-$pkgver/opt/$_pkgname/resources/." "$pkgdir/opt/$pkgname/"

    chmod 755 "$pkgdir/opt/$pkgname/$pkgname"

    install -d "$pkgdir/usr/bin/"
    ln -s "/opt/$pkgname/$pkgname" "$pkgdir/usr/bin/$pkgname"

    install -d "$pkgdir/usr/share/applications/"
    ln -s "/opt/$pkgname/$pkgname.desktop" "$pkgdir/usr/share/applications/$pkgname.desktop"

    install -d "$pkgdir/usr/share/icons/"
    ln -s "/opt/$pkgname/$pkgname.png" "$pkgdir/usr/share/icons/$pkgname.png"

    install -d "$pkgdir/usr/share/licenses/$pkgname/"
    ln -s "/opt/$pkgname/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
