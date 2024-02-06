# SPDX-License-Identifier: AGPL-3.0
#
# Contributor: David Runge <dvzrv@archlinux.org>
# Maintainer:  Pellegrino Prevete <cGVsbGVncmlub3ByZXZldGVAZ21haWwuY29tCg== | base -d>
# Maintainer:  Truocolo <truocolo@aol.com>

_build=true
_py="python2"
_proj="flit"
_pkg="${_proj}-core"
_Pkg="${_proj}_core"
[[ "${_py}" == python2 ]] && \
 _build=false
pkgname="${_py}-${_pkg}"
pkgver=3.9.0
pkgrel=1
pkgdesc="A PEP 517 build backend for packages using Flit"
arch=(
  any
)
_ns="pypa"
url="https://github.com/${_ns}/${_proj}flit/tree/main/${_Pkg}"
license=(
  BSD
)
depends=(
  "${_py}"
)
makedepends=(
  "${_py}-wheel"
)
[[ "${_build}" == true ]] && \
  makedepends+=(
    "${_py}-build"
    "${_py}-installer"
)
checkdepends=(
  "${_py}-pytest"
  "${_py}-testpath"
)
_pypi="https://files.pythonhosted.org/packages/source"
source=(
  "${_pypi}/${_pkg::1}/$_pkg}/${_pkg/-/_}-$pkgver.tar.gz"
)
sha512sums=(
  '1205589930d2c51d6aa6b2533a122a912e63b157e94adba2a0649a58d324fa98a5b84609d9b53e9d236f1cdb6a6984de2cefcf2f11abc2cd83956df21f269ad6'
)
b2sums=(
  '2fb053655a494736f5f9ce2d2c193d5d98622e410c0c0f18c92eb62d32ff98cbe830a1728461ed7e7e087d2fcf5f6a0c912717c2d534be688d688c4714c6865b'
)

build() {
  local \
    _module_opts=()
  _module_opts=(
    -m
      "build"
    pack
  )
  [[ "${_build}" == "true" ]] && \
    _module_opts=(
      -m
        build 
      --wheel
      --skip-dependency-check
      --no-isolation
    )
  cd \
    "${_pkg/-/_}-${pkgver}"
  "${_py}" \
    "${_module_opts[@]}"
}

check() {
  cd \
    "${_pkg/-/_}-${pkgver}"
  pytest \
    -vv
}

package() {
  local \
    _site_packages
  _site_packages=$( \
      "${_py}" \
        -c \
	  "import site; print(site.getsitepackages()[0])")
  cd \
    "${_pkg/-/_}-${pkgver}"
  "${_py}" \
    -m \
      installer \
      --destdir="${pkgdir}" \
      dist/*.whl
  install \
    -vDm 644 \
    LICENSE \
    -t \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  # remove tests
  rm \
    -frv \
    "${pkgdir}/${site_packages}/${_pkg/-/_}/tests/"
  # remove vendored tomli
  rm \
    -frv \
    "${pkgdir}/${site_packages}/${_pkg/-/_}/vendor/"
}

# vim:set sw=2 sts=-1 et:
