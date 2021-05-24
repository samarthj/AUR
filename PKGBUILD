# Maintainer: Renato Caldas <renato dat calgera ot com>
_pkgname=pdm-pep517
pkgname=python-$_pkgname
pkgver=0.7.4
pkgrel=1
pkgdesc="PEP 517 support for PDM"
arch=('any')
url="https://pdm.fming.dev/"
license=('MIT')
depends=('python')
makedepends=('python-build' 'python-pip')
source=("https://files.pythonhosted.org/packages/source/${_pkgname::1}/$_pkgname/$_pkgname-$pkgver.tar.gz")
sha512sums=('9eb8a766459bfe1c534dd076696103fef9d6eb6a2df510836d7ea65a6ca6eecaba17ad23bbeb25eca5f99d1f9f1cb0f74d35c1e213cbc8334d8df0211c751e23')

build() {
    cd $_pkgname-$pkgver
    python -m build --wheel .
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
    install -Dm644 LICENSE "${pkgdir}/usr/share/licenses/$pkgname/LICENSE"
}
