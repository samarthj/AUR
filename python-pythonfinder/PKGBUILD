# Maintainer: Sam  <dev at samarthj dot com>
# Contributor: Renato Caldas <renato dat calgera ot com>

# shellcheck disable=2034,2148,2154

_pkgname=pythonfinder
pkgname=python-pythonfinder
pkgver=1.2.9
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
sha512sums=('e461b36d160e42901519054159e03c3febadc1515e0ef6059ab13208073a91c9b968ad59f8873b0c39abb6466c3aa0b7b921995588e2f64256114ec0b9dc1097')

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
