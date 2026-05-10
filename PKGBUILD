# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Contributor: Dave Reisner <dreisner@archlinux.org>
# Contributor: Jaroslav Lichtblau <dragonlord@aur.archlinux.org>
# Contributor: Douglas Soares de Andrade
# Contributor: Roberto Alsina <ralsina@kde.org>

pkgname=vinyl-cache
pkgver=9.0.0
_pkg_vinyl_cache_commit=33ed181ff6
pkgrel=1
pkgdesc="High-performance HTTP accelerator"
arch=('x86_64')
url="https://www.vinyl-cache.org/"
license=('BSD-2-Clause')
# Yes, it really does need gcc during runtime to compile its rules.
depends=('gcc' 'gcc-libs' 'libnsl' 'pcre2')
makedepends=('python-docutils' 'python-sphinx' 'git')
optdepends=('python: needed for vmod development')
replaces=(varnish)
conflicts=(varnish)
backup=('etc/vinyl-cache/default.vcl')
install=$pkgname.install
source=("https://vinyl-cache.org/downloads/vinyl-cache-$pkgver.tgz"

        "git+https://code.vinyl-cache.org/vinyl-cache/pkg-vinyl-cache.git#commit=$_pkg_vinyl_cache_commit"
        )
sha512sums=('c7be55b13609a0842bb938af39203b77a1ed17257e18f7ea29b89760687317c135dee48b8b2936561c00aeabe321e503b209b5b9af876588dd824c6c6847d17e'
            'a23bcfdd097ba422f4960468dcc424eb5a7895cfc10ae194f0c22081b34b1d37e5a109ca26b36b74c8354b5a63cf9f24ac439b8a7b9af044a328712f7f3643d4')

build() {
  cd "$srcdir/$pkgname-$pkgver"

  ./configure \
    --prefix=/usr \
    --sysconfdir=/etc \
    --localstatedir=/var/lib \
    --sbindir=/usr/bin

  make
}

check() {
  cd "$srcdir/$pkgname-$pkgver"

  rm bin/vinyltest/tests/m00000.vtc
  make check
}

package() {
  cd "$srcdir/$pkgname-$pkgver"

  make DESTDIR="$pkgdir" install

  # NOTE: Wait for upstream to finish the rebranding?
  install -Dm644 "$srcdir/pkg-vinyl-cache/systemd/vinyl-cache.service" "$pkgdir/usr/lib/systemd/system/vinyl-cache.service"
  install -Dm644 "$srcdir/pkg-vinyl-cache/systemd/vinylncsa.service" "$pkgdir/usr/lib/systemd/system/varnishncsa.service"
  install -Dm755 "$srcdir/pkg-vinyl-cache/systemd/vinylreload" "$pkgdir/usr/bin/vinylreload"
  install -Dm755 "$srcdir/pkg-vinyl-cache/systemd/vinyl-cache.logrotate" "$pkgdir/etc/logrotate.d/vinyl-cache"
  install -Dm755 "$srcdir/pkg-vinyl-cache/systemd/vinyl-cache.sysusers" "$pkgdir/usr/lib/sysusers.d/vinyl-cache.conf"
  install -Dm644 "$srcdir/pkg-vinyl-cache/systemd/vinyl-cache.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/vinyl-cache.conf"

  install -Dm644 "etc/example.vcl" "$pkgdir/etc/vinyl-cache/default.vcl"

  # license
  install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
