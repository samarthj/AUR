# Maintainer: Sam  <dev at samarthj dot com>
# Contributor: Renato Caldas <renato dat calgera ot com>

# shellcheck disable=2034,2148,2154

_pkgname=pythonfinder
pkgname=python-pythonfinder
pkgver=1.2.8
pkgrel=2
pkgdesc="Cross Platform Search Tool for Finding Pythons"
arch=("any")
url="https://github.com/sarugaku/pythonfinder"
license=("MIT")
depends=("python" "python-attrs" "python-cached-property" "python-click" "python-six" "python-packaging")
makedepends=("python-pip")
_pkgname_prefix="${_pkgname:0:1}"
_pkgname_underscored="${_pkgname//-/_}"
_pkgurl="https://files.pythonhosted.org/packages/py2.py3/$_pkgname_prefix/$_pkgname/$_pkgname_underscored-$pkgver-py2.py3-none-any.whl"
source=("$_pkgurl")
sha512sums=('ee8a8cc1aad8eb7c9def0963cce98d46e27ebb9c62be571c47a2b6d9c4f62d07c099cdd9a46bb4803d4970d1974ce0617e205f53db1747b6c9c7a11055a67566')

package() {
  cd "$srcdir" || exit
  PIP_CONFIG_FILE=/dev/null pip install \
    --root="$pkgdir" \
    --isolated \
    --ignore-installed \
    --no-deps \
    --no-compile \
    --no-warn-script-location \
    ${_pkgname//-/_}-$pkgver-py2.py3-none-any.whl
  python -O -m compileall -s "$pkgdir" "$pkgdir/usr/lib"
  mapfile -t direct_url_file < <(find "$pkgdir"/usr/lib -type f -name 'direct_url.json')
  rm -rf "${direct_url_file[@]}" || true
  install -Dm644 "${_pkgname//-/_}-$pkgver.dist-info/LICENSE.txt" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
