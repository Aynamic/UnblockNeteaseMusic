pkgname=unblock-netease-music
pkgver=0.25.3
pkgrel=4
pkgdesc="Revive unavailable songs for Netease Cloud Music"
arch=(any)
url=https://github.com/nondanee/UnblockNeteaseMusic
license=(MIT)
depends=(nodejs)
makedepends=(npm jq)
source=(
	$pkgname-$pkgver.tar.gz::$url/archive/v$pkgver.tar.gz
	UnblockNeteaseMusic.service
)
sha512sums=('63425da7a0a7419dcd9e12f620601eebdedeb40016af876b44779f78f8a0c586fa97599076b61948a476cba3439f4da8f7e2121d8ec216882c580a848ca18898'
            '0454794e57c35f669023a9d4189de19eebae926e826e4b2095191beb359c2d0ab7dced85b231c2f5a36d23baf777fab2ffeb2250989663a71a5a1d7d03f23d09')
package() {
	npm install -g --user root --prefix "$pkgdir/usr" -cache  "$srcdir/npm-cache" "$srcdir/$pkgname-$pkgver.tar.gz"
	find "$pkgdir/usr" -type d -exec chmod 755 {} \;
	chown -R root:root "$pkgdir"
	find "$pkgdir" -name package.json -print0 | xargs -r -0 sed -i "/_where/d"
	local tmppackage="$(mktemp)"
	local pkgjson="$pkgdir/usr/lib/node_modules/@nondanee/unblockneteasemusic/package.json"
	jq '.|=with_entries(select(.key|test("_.+")|not))' "$pkgjson" > "$tmppackage"
	mv "$tmppackage" "$pkgjson"
	chmod 644 "$pkgjson"

	install -Dm644 "$srcdir/UnblockNeteaseMusic.service" "$pkgdir/usr/lib/systemd/system/UnblockNeteaseMusic.service"
}
