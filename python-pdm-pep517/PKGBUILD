# Maintainer: Sam  <dev at samarthj dot com>
# Contributor: Renato Caldas <renato dat calgera ot com>

# shellcheck disable=2034,2148,2154

_pkgname=pdm-pep517
pkgname=python-$_pkgname
pkgver=0.8.0
pkgrel=2
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
sha512sums=('08278cfc6826276a79c72f383f92af21f1c6e17dacd13d605fd74da592df1e6bddfe200d3062a679a3a427b5bf9d7110de9b1bfb1402b11ef2a7031412478249')

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
  python -O -m compileall -j "$(nproc)" -s "$pkgdir" "$pkgdir/usr/lib/"
  mapfile -t direct_url_file < <(find "$pkgdir"/usr/lib -type f -name 'direct_url.json')
  rm -rf "${direct_url_file[@]}" || true
  install -Dm644 "${_pkgname//-/_}-$pkgver.dist-info/LICENSE" "$pkgdir/usr/share/licenses/$pkgname/LICENSE"
}
