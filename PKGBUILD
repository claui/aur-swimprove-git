# Maintainer: Claudia Pellegrino <aur ät cpellegrino.de>

pkgname=swimprove-git
pkgver=r7.ab39ff8
pkgrel=1
pkgdesc='Helpers for my personal Sway configuration'
arch=('any')
url='https://github.com/claui/swimprove'
license=('Apache')
depends=('i3status' 'jq' 'sudo' 'systemd')
makedepends=('git')
optdepends=(
  'ectool: Data source for the fan speed'
  'i3-wm: Window manager to host the custom status bar'
  'sway: Wayland compositor to host the custom status bar'
)
conflicts=('swimprove')
options=('!strip')
source=("${pkgname}::git+https://github.com/claui/swimprove.git")
sha512sums=('SKIP')

pkgver() {
  printf "r%s.%s" \
    "$(git -C "${pkgname}" rev-list --count HEAD)" \
    "$(git -C "${pkgname}" rev-parse --short HEAD)"
}

build() {
  echo >&2 'Preparing the binstub'
  mkdir -p "${srcdir}"
  printf '#!/bin/bash\n%s\n' > "${srcdir}/binstub" \
    'exec "/usr/lib/'"${pkgname}"'/bin/$(basename "${0}")" "$@"'
}

package() {
  echo >&2 'Packaging the license'
  install -D -m 644 -t "${pkgdir}/usr/share/licenses/${pkgname}" \
    "${srcdir}/${pkgname}/LICENSE"

  echo >&2 'Installing package files'
  mkdir -p "${pkgdir}/usr/lib/${pkgname}"
  cp -r --preserve=mode -t "${pkgdir}/usr/lib/${pkgname}" \
    "${srcdir}/${pkgname}/"{bin,libexec}

  echo >&2 'Installing binstubs'
  find "${srcdir}/${pkgname}/bin" \
    -mindepth 1 \
    -exec bash -c "install -D -m 755 -T \"${srcdir}\"/binstub`
      ` \"${pkgdir}\"/usr/bin/\$(basename '{}')" ';'

  echo >&2 'Installing documentation'
  install -D -m 644 -t "${pkgdir}/usr/share/doc/${pkgname}" \
    "${srcdir}/${pkgname}/README.md"
}
