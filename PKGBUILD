# Maintainer: Renato Caldas <renato dat calgera ot com>
_pkgname=pythonfinder
pkgname=python-pythonfinder
pkgver=1.2.6
pkgrel=2
pkgdesc="Cross Platform Search Tool for Finding Pythons"
arch=('any')
url="Cross Platform Search Tool for Finding Pythons"
license=('MIT')
depends=('python' 'python-attrs' 'python-cached-property' 'python-click' 'python-six' 'python-packaging')
makedepends=('python-setuptools')
#checkdepends=('python-pytest' 'python-pytest-timeout' 'python-pytest-flake8' 'python-pytest-cov')
source=("$_pkgname-$pkgver.tar.gz::https://github.com/sarugaku/$_pkgname/archive/refs/tags/$pkgver.tar.gz")
sha512sums=('41ad53cbb886c4a8197f3975993cb2e10c644dee87420060661d51d503bf2163f45db7e9ab9c343a3a94b2936e63e960099be13055ba6f37462e6a208e39e0f6')

build() {
    cd $_pkgname-$pkgver
    python setup.py build
}

# This seems to be broken upstream
# check() {
#     cd $_pkgname-$pkgver
#     pytest
# }

package() {
    cd $_pkgname-$pkgver
    python setup.py install --root="$pkgdir" --optimize=1 --skip-build
    install -Dm644 LICENSE.txt "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
