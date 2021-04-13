# Maintainer: Renato Caldas <renato dat calgera ot com>
_pkgname=pythonfinder
pkgname=python-pythonfinder
pkgver=1.2.6
pkgrel=1
pkgdesc="Cross Platform Search Tool for Finding Pythons"
arch=('any')
url="Cross Platform Search Tool for Finding Pythons"
license=('MIT')
depends=('python' 'python-attrs' 'python-cached-property' 'python-click' 'python-six' 'python-packaging')
makedepends=('python-setuptools')
#checkdepends=('python-pytest' 'python-pytest-timeout' 'python-pytest-flake8' 'python-pytest-cov')
source=("$_pkgname-$pkgver.tar.gz::https://github.com/sarugaku/$_pkgname/archive/refs/tags/$pkgver.tar.gz")
sha512sums=('1b095d31d0932944ce7eb74476626c8a6cc9afca65df082daafd374695a97d666574a63416d79d0e0b1feb4bdcd8fcc93be6b089b685a549d519942b9c4fc1e2')

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
