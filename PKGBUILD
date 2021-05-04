# Maintainer: Renato Caldas <renato dat calgera ot com>
_pkgname=pdm-pep517
pkgname=python-$_pkgname
pkgver=0.7.2
pkgrel=1
pkgdesc="PEP 517 support for PDM"
arch=('any')
url="https://pdm.fming.dev/"
license=('MIT')
depends=('python')
makedepends=('python-build' 'python-pip')
source=("https://files.pythonhosted.org/packages/source/${_pkgname::1}/$_pkgname/$_pkgname-$pkgver.tar.gz")
sha512sums=('ffb82060d0cc3b32eac6a9e144d14d5d81fd859ee779614f09902c1bfc660748e4ceb683749e0188b1c163ea743d6e813217cc563a761a3dd2ff488cee7589be')

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
