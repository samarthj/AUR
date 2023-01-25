# Maintainer: Sam  <dev at samarthj dot com>
# Contributor: Renato Caldas <renato dat calgera ot com>

# shellcheck disable=2034,2148,2154

pkgname=python-pdm-pep517
_name=${pkgname#python-}
pkgver=1.0.0
pkgrel=1
pkgdesc="PEP 517 support for PDM"
arch=("any")
url="https://github.com/pdm-project/pdm-pep517"
license=("MIT")
depends=("python>=3.7" "python-boolean.py>=3.8" "python-cerberus>=1.3.4" "python-license-expression>=21.6.14" "python-packaging>=21.0" "python-pyparsing>=3.0.7" "python-tomli>=2.0.1" "python-tomli-w>=1.0.0")
makedepends=("python-build" "python-installer")
checkdepends=("git" "python-pytest")
source=("https://files.pythonhosted.org/packages/source/${_name::1}/${_name}/${_name}-${pkgver}.tar.gz")
sha512sums=('7dc0d2954d1804f6e013537ef33ef0f90498f4036dcd87d75a33fa443cbe9cfe3432c8d087b84498acbd23391d4aeab0e43c53055d4745ad105e9e6767531aeb')

prepare() {
  cd "$srcdir/${_name}-$pkgver" || exit 1
  _devendored=(boolean cerberus license_expression packaging pyparsing tomli tomli_w)
  for p in "${_devendored[@]}"; do
    echo "devendoring python-${p//_/-}"
    rm -frv pdm/pep517/_vendor/"${p}"*
    sed -i "s/^${p//_/-}==.*$//g" pdm/pep517/_vendor/vendor.txt
    sed -i "s/from pdm.pep517._vendor import ${p}/import ${p}/g" pdm/pep517/*.py tests/*.py
    sed -i "s/from pdm.pep517._vendor.${p} import/from ${p} import/g" pdm/pep517/*.py tests/*.py
    sed -i "s/from pdm.pep517._vendor.${p}.\(.*\) import/from ${p}.\1 import/g" pdm/pep517/*.py tests/*.py
    sed -i "s/import pdm.pep517._vendor.${p}/import ${p}/g" pdm/pep517/*.py tests/*.py
  done
}

build() {
  cd "$srcdir/${_name}-$pkgver" || exit 1
  python -m build --wheel --skip-dependency-check --no-isolation
}

check() {
  cd "$srcdir/${_name}-$pkgver" || exit 1

  # set default git config for test
  git config --global --get "user.email" || git config --global user.email "test@example.com"
  git config --global --get "user.name" || git config --global user.name "test"
  pytest -vvv tests
}

package() {
  cd "$srcdir/${_name}-$pkgver" || exit 1
  python -m installer --destdir="$pkgdir" dist/*.whl
  install -Dm644 "$srcdir/${_name}-$pkgver/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
  install -vDm 644 "$srcdir/${_name}-$pkgver/README.md" -t "$pkgdir/usr/share/doc/$pkgname/README.md"
}
