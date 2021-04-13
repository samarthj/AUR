# Maintainer: Renato Caldas <renato dat calgera ot com>
_pkgname=pdm-pep517
pkgname=python-$_pkgname
pkgver=0.6.1
pkgrel=1
pkgdesc="PEP 517 support for PDM"
arch=('any')
url="https://pdm.fming.dev/"
license=('MIT')
depends=('python')
makedepends=('python-build' 'python-pip')
checkdepends=('git' 'python-pytest')
source=("$_pkgname-$pkgver.tar.gz::https://github.com/pdm-project/$_pkgname/archive/refs/tags/$pkgver.zip")
sha512sums=('7dc39e24414aaa7bd2095c8739836e0e73b9758c3354559ca4b7c9a956c37404aff1de3346ffb1b804d2bcbe63073d805d681ce0cc77360450435ebc9a46b94d')

build() {
    cd $_pkgname-$pkgver
    python -m build --no-isolation --wheel .
}

check() {
    cd $_pkgname-$pkgver
    pytest
}

package() {
    cd $_pkgname-$pkgver
    PIP_CONFIG_FILE=/dev/null pip install \
      --root="$pkgdir" \
      --isolated \
      --ignore-installed \
      --no-deps \
      --no-compile \
      dist/${_pkgname//-/_}-$pkgver-py3-none-any.whl
    # pip compilation includes $pkgdir paths in the compiled files, this works around that:
    python -O -m compileall "${pkgdir}/$pkgname"
    install -Dm644 LICENSE "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
