# Maintainer: Hyacinthe Cartiaux <hyacinthe.cartiaux@free.fr>
# Maintainer: Eli Schwartz <eschwartz93@gmail.com>

# All my PKGBUILDs are managed at https://github.com/eli-schwartz/pkgbuilds

_pkgname=https-everywhere
pkgname=firefox-extension-${_pkgname}
pkgver=5.2.1
pkgrel=1
_file=471033
pkgdesc="Plugin for firefox which ensures you are using https whenever it's possible."
license=('GPL2')
arch=('any')
url="https://www.eff.org/https-everywhere"
depends=("firefox")
makedepends=("unzip")
source=("${_pkgname}-${pkgver}.xpi::https://addons.mozilla.org/firefox/downloads/file/${_file}/${_pkgname/-/_}-${pkgver}-an+tb+sm+fx.xpi")
noextract=("${_pkgname}-${pkgver}.xpi")
sha256sums=('5a9c9cd9ca93752d9bfbbf93227c572c5756807b441742b2749b064a4b486068')

prepare() {
  cd "$srcdir"

  unzip -qqo "${_pkgname}-${pkgver}.xpi" -d "${_pkgname}-${pkgver}"
}

package() {
  cd "${srcdir}"

  _signdata="${_pkgname}-${pkgver}/META-INF/mozilla.rsa"
  if [[ ! -f "${_signdata}" ]] || ! grep -qs Production1 "${_signdata}"; then
    warning "No valid signing data from AMO. This extension will *NOT* work unless you disable extension verification in Firefox."
    # Add install file to print warning.
    install=firefox-extension-verification.install
  fi

  _extension_id="$(sed -n '/.*<em:id>\(.*\)<\/em:id>.*/{s//\1/p;q}' ${_pkgname}-${pkgver}/install.rdf)"
  _extension_dest="${pkgdir}/usr/lib/firefox/browser/extensions/${_extension_id}"
  # Should this extension be unpacked or not?
  if grep '<em:unpack>true</em:unpack>' ${_pkgname}-${pkgver}/install.rdf > /dev/null; then
    install -dm755 "${_extension_dest}"
    cp -R ${_pkgname}-${pkgver}/* "${_extension_dest}"
    chmod -R ugo+rX "${_extension_dest}"
  else
    install -Dm644 ${_pkgname}-${pkgver}.xpi "${_extension_dest}.xpi"
  fi
}
