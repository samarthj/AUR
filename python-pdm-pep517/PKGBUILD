# Maintainer: Sam  <dev at samarthj dot com>
# Contributor: Renato Caldas <renato dat calgera ot com>

# shellcheck disable=2034,2148,2154

_pkgname=pdm-pep517
pkgname=python-$_pkgname
pkgver=0.9.3
pkgrel=3
pkgdesc="PEP 517 support for PDM"
arch=("any")
url="https://pdm.fming.dev/"
license=("MIT")
depends=("python")
makedepends=("python-pip")
_pkgname_prefix="${_pkgname:0:1}"
_pkgname_underscored="${_pkgname//-/_}"
_pkgurl="https://files.pythonhosted.org/packages/py3/$_pkgname_prefix/$_pkgname/$_pkgname_underscored-$pkgver-py3-none-any.whl"
source=("$_pkgurl")
sha512sums=('095aec85c7a48cf3f249e77f40840c1dc2a86708f7bbc919ee7e120f40b84bf1e6b6b78c8cb45e4b3218e4bd591ff562df3d482c48352dbb6f1f59b7dbc86073')

package() {
  cd "$srcdir" || exit
  PIP_CONFIG_FILE=/dev/null pip install \
    --root="$pkgdir" \
    --isolated \
    --ignore-installed \
    --no-deps \
    --no-compile \
    --no-warn-script-location \
    ${_pkgname//-/_}-$pkgver-py3-none-any.whl
  python -O -m compileall -s "$pkgdir" "$pkgdir/usr/lib/"
  mapfile -t direct_url_file < <(find "$pkgdir"/usr/lib -type f -name 'direct_url.json')
  rm -rf "${direct_url_file[@]}" || true
  install -Dm644 "${_pkgname//-/_}-$pkgver.dist-info/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
