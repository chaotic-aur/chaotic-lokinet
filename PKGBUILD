# Maintainer: jason <jason@oxen.io>
pkgname=lokinet
pkgver=0.9.9
pkgrel=1
pkgdesc="Anonymous, decentralized and IP based overlay network for the internet."
arch=('x86_64' 'aarch64')
url="https://lokinet.org"
license=('GPL3')
depends=('libuv' 'libsodium' 'curl' 'unbound' 'sqlite' 'jemalloc' 'libsystemd' 'zeromq')
makedepends=('git' 'cmake' 'pkgconf')
conflicts=('lokinet-bin')
source=("$pkgname::git+https://github.com/oxen-io/lokinet#branch=makepkg")
b2sums=('SKIP')

prepare() {
          cd "$srcdir/$pkgname"
          git submodule update --init --recursive
}

build() {
    cd "$srcdir/$pkgname"

    rm -rf build && mkdir build && cd build
    cmake \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_C_FLAGS="$CFLAGS" \
        -DCMAKE_CXX_FLAGS="$CXXFLAGS" \
        -DFORCE_OXENMQ_SUBMODULE=ON \
        -DFORCE_OXENC_SUBMODULE=ON \
        -DWITH_TESTS=OFF \
        -DWITH_SYSTEMD=ON \
        -DWITH_SETCAP=OFF \
        -DBUILD_LIBLOKINET=OFF \
        ..
    make
}

package() {
    cd "$srcdir/$pkgname"
    install -D -m 644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
    cd build
    make DESTDIR="$pkgdir" install

    install -D -m 644 "$srcdir/$pkgname/contrib/archlinux/lokinet.service"                "$pkgdir/usr/lib/systemd/system/lokinet.service"
    install -D -m 644 "$srcdir/$pkgname/contrib/archlinux/lokinet-vpn@.service"           "$pkgdir/usr/lib/systemd/system/lokinet-vpn@.service"
    install -D -m 644 "$srcdir/$pkgname/contrib/archlinux/lokinet-bootstrap.service"      "$pkgdir/usr/lib/systemd/system/lokinet-bootstrap.service"
    install -D -m 644 "$srcdir/$pkgname/contrib/archlinux/lokinet-default-config.service" "$pkgdir/usr/lib/systemd/system/lokinet-default-config.service"
    install -D -m 644 "$srcdir/$pkgname/contrib/archlinux/lokinet-resume.service"         "$pkgdir/usr/lib/systemd/system/lokinet-resume.service"
    install -D -m 644 "$srcdir/$pkgname/contrib/archlinux/lokinet.sysusers"               "$pkgdir/usr/lib/sysusers.d/lokinet.conf"
    install -D -m 644 "$srcdir/$pkgname/contrib/archlinux/lokinet.tmpfiles"               "$pkgdir/usr/lib/tmpfiles.d/lokinet.conf"
    install -D -m 644 "$srcdir/$pkgname/contrib/systemd-resolved/lokinet.pkla"            "$pkgdir/var/lib/polkit-1/localauthority/10-vendor.d/lokinet.pkla"
    install -D -m 750 -d "$pkgdir/usr/share/polkit-1/rules.d"
    install -D -m 644 "$srcdir/$pkgname/contrib/systemd-resolved/lokinet.rules"           "$pkgdir/usr/share/polkit-1/rules.d/lokinet.rules"
}
