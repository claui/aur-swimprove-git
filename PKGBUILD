# Maintainer: Claudia Pellegrino <aur ät cpellegrino.de>

pkgname=swimprove-git
pkgver=r7.ab39ff8
pkgrel=2
pkgdesc='Helpers for my personal Sway configuration'
arch=('any')
url='https://github.com/claui/swimprove'
license=('Apache-2.0')
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

prepare() {
  cd "${srcdir}/${pkgname}"
  echo >&2 'Removing unneeded files'
  for dir in 'bin' 'libexec'; do
    find "${dir?}" -name '.*' -exec rm -fv '{}' +
  done

  echo >&2 'Preparing the binstub'
  # shellcheck disable=SC2016  # This isn’t supposed to expand at build time
  printf > 'binstub' \
    '#!/bin/bash\nexec "/usr/lib/%s/bin/$(basename "${0}")" "$@"\n' \
    "${pkgname}"
}

package() {
  cd "${srcdir}/${pkgname}"
  echo >&2 'Packaging the license'
  install -D -m 644 -t "${pkgdir}/usr/share/licenses/${pkgname}" \
    'LICENSE'

  echo >&2 'Packaging library files and internal binstubs'
  mkdir -p "${pkgdir}/usr/lib/${pkgname}"
  cp -r --preserve=mode -t "${pkgdir}/usr/lib/${pkgname}" \
    'bin' 'libexec'

  echo >&2 'Packaging external binstubs'
  find 'bin' -mindepth 1 -exec bash -c \
    'install -D -m 755 -T "${1}" "${2}/$(basename "${3}")"' \
    _ 'binstub' "${pkgdir}/usr/bin" '{}' ';'

  echo >&2 'Packaging documentation'
  install -D -m 644 -t "${pkgdir}/usr/share/doc/${pkgname}" \
    'README.md'
}
