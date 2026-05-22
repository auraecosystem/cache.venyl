# Maintainer: Sven-Hendrik Haase <svenstaro@archlinux.org>
# Contributor: Dave Reisner <dreisner@archlinux.org>
# Contributor: Jaroslav Lichtblau <dragonlord@aur.archlinux.org>
# Contributor: Douglas Soares de Andrade
# Contributor: Roberto Alsina <ralsina@kde.org>

pkgname=vinyl-cache
pkgver=9.0.1
_pkg_vinyl_cache_commit=33ed181ff6
pkgrel=1
pkgdesc="High-performance HTTP accelerator"
arch=('x86_64')
url="https://www.vinyl-cache.org/"
license=('BSD-2-Clause')
# Yes, it really does need gcc during runtime to compile its rules.
depends=('gcc' 'libnsl' 'pcre2' 'glibc' 'libgcc' 'readline')
makedepends=('python-docutils' 'python-sphinx' 'git')
optdepends=('python: needed for vmod development')
replaces=(varnish)
conflicts=(varnish)
backup=('etc/vinyl-cache/default.vcl')
install=$pkgname.install
source=("https://vinyl-cache.org/downloads/vinyl-cache-$pkgver.tgz"
        "git+https://code.vinyl-cache.org/vinyl-cache/pkg-vinyl-cache.git#commit=$_pkg_vinyl_cache_commit"
        ${pkgname}-fix-destdir.patch::https://code.vinyl-cache.org/vinyl-cache/vinyl-cache/commit/4196617a1817a29ddf54a7a666099aa02fe510f1.patch)
sha512sums=('6aaa936e3bce6e195d469642949bf35a76cd04e6bcdc6ff1bc608ce2b37c871cab50f779918331658aefb853e78a71336ac5cf95e49524807a2929337940ab8e'
            'a23bcfdd097ba422f4960468dcc424eb5a7895cfc10ae194f0c22081b34b1d37e5a109ca26b36b74c8354b5a63cf9f24ac439b8a7b9af044a328712f7f3643d4'
            '46494a8b865368f43a7068735379d7b553d11e61cf2f49efcf2b983ae65ad44da5146bb46419db20f0608a0c1a6d8187ad562e2f6adc6a098c25c310e1da67a4')

prepare() {
  cd "$srcdir/$pkgname-$pkgver"

  # Fix install-data-local to respect DESTDIR
  patch -Np1 -i "${srcdir}"/${pkgname}-fix-destdir.patch
}

build() {
  cd "$srcdir/$pkgname-$pkgver"

  autoreconf -i
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

  install -Dm644 "$srcdir/pkg-vinyl-cache/systemd/vinyl-cache.service" "$pkgdir/usr/lib/systemd/system/vinyl-cache.service"
  install -Dm644 "$srcdir/pkg-vinyl-cache/systemd/vinylncsa.service" "$pkgdir/usr/lib/systemd/system/vinylncsa.service"
  install -Dm755 "$srcdir/pkg-vinyl-cache/systemd/vinylreload" "$pkgdir/usr/bin/vinylreload"
  install -Dm755 "$srcdir/pkg-vinyl-cache/systemd/vinyl-cache.logrotate" "$pkgdir/etc/logrotate.d/vinyl-cache"
  install -Dm755 "$srcdir/pkg-vinyl-cache/systemd/vinyl-cache.sysusers" "$pkgdir/usr/lib/sysusers.d/vinyl-cache.conf"
  install -Dm644 "$srcdir/pkg-vinyl-cache/systemd/vinyl-cache.tmpfiles" "$pkgdir/usr/lib/tmpfiles.d/vinyl-cache.conf"

  install -Dm644 "etc/example.vcl" "$pkgdir/etc/vinyl-cache/default.vcl"

  install -Dm644 "LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
