# SPDX-License-Identifier: AGPL-3.0

#    ----------------------------------------------------------------------
#    Copyright © 2023, 2024, 2025  Pellegrino Prevete
#
#    All rights reserved
#    ----------------------------------------------------------------------
#
#    This program is free software: you can redistribute it and/or modify
#    it under the terms of the GNU Affero General Public License as published by
#    the Free Software Foundation, either version 3 of the License, or
#    (at your option) any later version.
#
#    This program is distributed in the hope that it will be useful,
#    but WITHOUT ANY WARRANTY; without even the implied warranty of
#    MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
#    GNU Affero General Public License for more details.
#
#    You should have received a copy of the GNU Affero General Public License
#    along with this program.  If not, see <https://www.gnu.org/licenses/>.

# Maintainer:
#   Truocolo
#     <truocolo@aol.com>
#     <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
# Maintainer:
#   Pellegrino Prevete (dvorak)
#     <pellegrinoprevete@gmail.com>
#     <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
# Maintainer:
#   David (ReyJamonico)
#     < david at rjamo dot dev >
# Contributor:
#   Viktor Drobot (aka dviktor)
#     linux776 [at] gmail [dot] com
# Contributor:
#   Sven-Hendrik Haase
#     <svenstaro@gmail.com>
# Contributor:
#   Konstantin Gizdov
#     <arch@kge.pw>
# Contributor:
#   Bartłomiej Piotrowski
#     <bpiotrowski@archlinux.org>
# Contributor:
#   Allan McRae
#     <allan@archlinux.org>

# toolchain build order:
# linux-api-headers -> 
#   glibc -> 
#     binutils ->
#       gcc ->
#         binutils ->
#           glibc
# NOTE:
#   libtool requires rebuilt
#   with each new gcc version

_evmfs_available="$( \
  command \
    -v \
    "evmfs" || \
    true)"
if [[ ! -v "_evmfs" ]]; then
  if [[ "${_evmfs_available}" != "" ]]; then
    _evmfs="true"
  elif [[ "${_evmfs_available}" == "" ]]; then
    _evmfs="false"
  fi
fi
_os="$( \
  uname \
    -o)"
if [[ "${_os}" == "Android" ]]; then
  _libc="ndk-sysroot"
elif [[ "${_os}" == "GNU/Linux" ]]; then
  _libc="glibc"
  _libc_ver="2.27"
fi
if [[ ! -v "_docs" ]]; then
  _docs="false"
fi
_py="python"
_proj="gnu"
_pkg=gcc
_majver=7
pkgbase="${_pkg}${_majver}"
pkgname=(
  "${pkgbase}"
  "${pkgbase}-libs"
  "${pkgbase}-fortran"
)
if [[ "${_docs}" == "true" ]]; then
  pkgname+=(
    "${pkgbase}-docs"
  )
fi
pkgver="${_majver}.5.0"
_pkgver="${_majver}"
_majorver=${pkgver:0:1}
_islver=0.18
pkgrel=5
_pkgdesc=(
  'The GNU Compiler Collection (7.x.x)'
)
pkgdesc="${_pkgdesc[*]}"
arch=(
  'arm'
  'aarch64'
  'x86_64'
)
license=(
  'GPL'
  'LGPL'
  'FDL'
  'custom'
)
_domain="${_pkg}.${_proj}.org"
_http="https://${_domain}"
url="${_http}"
depends=(
  "${_libc}"
)
makedepends=(
  'binutils'
  'libmpc'
  "make"
  "${_py}"
  'subversion'
  'flex'
)
if [[ "${_docs}" == "true" ]]; then
  makedepends+=(
    "doxygen"
  )
fi
options=(
  "!emptydirs"
  "!lto"
)
_ns="pub"
_url="${_http}/${_ns}/${_pkg}"
_tarname="${_pkg}-${pkgver}"
source=(
  "${_url}/releases/${_tarname}/${_tarname}.tar.xz"
  "${_url}/infrastructure/isl-${_islver}.tar.bz2"
  "bz84080.patch"
  "libsanitizer.patch"
)
sha256sums=(
  'b81946e7f01f90528a1f7352ab08cc602b9ccc05d4e44da4bd501c5a189ee661'
  '6b8b0fd7f81d0a957beb3679c81bbb34ccc7568d5682844d8924424a0dadcb1b'
  'bce05807443558db55f0d6b4dae37a678ea1bb3388b541c876fe3d110e3717e7'
  'ee25895428a9dbd3217de109a043c54cb9f51e6a04a260be088a619c0f677e68'
)
if [[ "${_evmfs}" == "true" ]]; then
  validpgpkeys=(
    # Truocolo
    #   <truocolo@aol.com>
    '97E989E6CF1D2C7F7A41FF9F95684DBE23D6A3E9'
    #   <truocolo@0x6E5163fC4BFc1511Dbe06bB605cc14a3e462332b>
    'F690CBC17BD1F53557290AF51FC17D540D0ADEED'
    # Pellegrino Prevete (dvorak)
    #   <dvorak@0x87003Bd6C074C713783df04f36517451fF34CBEf>
    '12D8E3D7888F741E89F86EE0FEC8567A644F1D16'
  )
fi
_svnrev=266882
_svn_ns="svn"
_svnurl="svn://${_domain}/${_svn_ns}/${_pkg}/branches/${_pkg}-${_majorver}-branch"
_arch="$( \
  uname \
    -m)"
if [[ "${_arch}" == "arm" ]]; then
  _arch="armv7l"
else
  if [[ -v "CARCH" ]]; then
    _arch="${CARCH}"
  fi
fi
_libdir="usr/lib/${_pkg}/${_arch}/${pkgver%%+*}"

snapshot() {
  local \
    _datestamp \
    _basever \
    _pkgver
  svn \
    export \
    -r"${_svnrev}" \
    "${_svnurl}" \
    "gcc-r${_svnrev}"
  _basever="$(< \
    "${_pkg}-r${_svnrev}/${_pkg}/BASE-VER")"
  _datestamp="$(< \
    "${_pkg}-r${_svnrev}/${_pkg}/DATESTAMP")"
  _pkgver="${_basever}-${_datestamp}"
  mv \
    "${_pkg}-r${_svnrev}" \
    "${_pkg}-${_pkgver}"
  tar \
    cf - \
    "gcc-${_pkgver}" | \
    xz > \
      "${_pkg}-${_pkgver}.tar.xz"
  rm \
    -rf \
    "${_pkg}-${_pkgver}"
  gpg \
    -b \
    "${_pkg}-${_pkgver}.tar.xz"
  scp \
    "${_pkg}-${_pkgver}.tar.xz"{"",".sig"} \
    "sources.archlinux.org:/srv/ftp/other/${_pkg}/"
  echo
  echo \
    "pkgver=${_pkgver/-/+}"
}

_usr_get() {
  local \
    _bin
  _bin="$( \
    dirname \
      "$(command \
           -v \
	   "env")")"
  dirname \
    "${_bin}"
}

prepare() {
  depends=(
    "${_libc}"
  )
  if [[ ! -d "${_pkg}" ]]; then
    ln \
      -s \
      "${_pkg}-${pkgver/+/-}" \
      "${_pkg}"
  fi
  cd \
    "${_pkg}"
  # https://gcc.gnu.org/bugzilla/show_bug.cgi?id=84080
  patch \
    -p0 \
    -i \
    "${srcdir}/bz84080.patch"
  # https://gcc.gnu.org/bugzilla/show_bug.cgi?id=92154
  patch \
    -Np0 \
    -i \
    "${srcdir}/libsanitizer.patch"
  # link isl for in-tree build
  ln \
    -s \
    "../isl-${_islver}" \
    "isl"
  # Do not run fixincludes
  sed \
    -i \
    's@\./fixinc\.sh@-c true@' \
    "${_pkg}/Makefile.in"
  # Arch Linux installs x86_64 libraries /lib
  sed \
    -i \
    '/m64=/s/lib64/lib/' \
    "gcc/config/i386/t-linux64"
  # hack:
  #   some configure tests
  #   for header files using "$CPP $CPPFLAGS"
  sed \
    -i \
    "/ac_cpp=/s/\$CPPFLAGS/\$CPPFLAGS -O2/" \
    {"libiberty","gcc"}"/configure"
  mkdir \
    -p \
    "$srcdir/${_pkg}-build"
}

build() {
  local \
    _banned_compile_opts=() \
    _configure \
    _configure_opts=() \
    _opt
  _configure="${srcdir}/${_pkg}/configure"
  _configure_opts=(
    --prefix="/usr"
    --libdir="/usr/lib"
    --libexecdir="/usr/lib"
    --mandir="/usr/share/man"
    --infodir="/usr/share/info"
    --with-bugurl="https://bugs.archlinux.org/"
    --enable-languages="c,c++,fortran,lto"
    --disable-multilib
    --enable-shared
    --enable-threads="posix"
    --enable-libmpx
    --with-system-zlib
    --with-isl
    --enable-__cxa_atexit
    --disable-libunwind-exceptions
    --enable-clocale="gnu"
    --disable-libstdcxx-pch
    --disable-libssp
    --enable-gnu-unique-object
    --enable-linker-build-id
    --enable-lto
    --enable-plugin
    --enable-install-libiberty
    --with-linker-hash-style="gnu"
    --enable-gnu-indirect-function
    --disable-werror
    --enable-checking="release"
    --enable-default-pie
    --enable-default-ssp
    --program-suffix="-${_pkgver}"
    --enable-version-specific-runtime-libs
  )
  _banned_compile_opts+=(
    "-pipe"
    "-Werror=format-security"
    "-fstack-clash-protection"
    "-fcf-protection"
  )
  export \
    LD_PRELOAD="/usr/lib/libstdc++.so"
  cd \
    "${_pkg}-build"
  # using -pipe causes spurious test-suite failures
  # http://gcc.gnu.org/bugzilla/show_bug.cgi?id=48565
  # -Werror=format-security causes compilation errors with GCC>10
  # And protection flags leave libgcc unusable
  for _opt in "${_banned_compile_options[@]}"; do
    CFLAGS="${CFLAGS/${_option}/}"
    CXXFLAGS="${CXXFLAGS/${_option}/}"
  done
  "${_configure}" \
    "${_configure_opts[@]}"
  make
  if [[ "${_docs}" == "true" ]]; then
    make \
      -C \
        "${CHOST}/libstdc++-v3/doc" \
      "doc-man-doxygen"
  fi
}

package_gcc7-libs() {
  local \
    _lib \
    _libc_depends \
    _os \
    _pkgdesc=()
  _libc_depends=""
  _os="$( \
    uname \
      -o)"
  _pkgdesc=(
    'Runtime libraries'
    'shipped by GCC (7.x.x)'
  )
  pkgdesc="${_pkgdesc[*]}"
  if [[ "${_os}" == "GNU/Linux" ]]; then
    _libc_depends="${_libc}>=${_libc_ver}"
  elif [[ "${_os}" == "Android" ]]; then
    _libc_depends="${_libc}"
  fi
  if [[ -v "${_libc_depends}" ]]; then
    echo "libc depends: ${_libc_depends}"
    depends=(
      "${_libc_depends}"  fi
    )                     
  fi
  echo "${depends[*]}"
  options+=(
    "!strip"
  )
  export \
    LD_PRELOAD="${_usr_get}/lib/libstdc++.so"
  cd \
    "${_pkg}-build"
  make \
    -C \
      "${CHOST}/lib${_pkg}" \
    DESTDIR="${pkgdir}" \
    install-shared
  rm \
    -f \
    "${pkgdir}/${_libdir}/lib${_pkg}_eh.a"
  mv \
    "${pkgdir}/usr/lib/${_pkg}/${CHOST}/lib/lib${_pkg}_s.so"* \
    "${pkgdir}/${_libdir}"
  for _lib in \
    "libatomic" \
    "libcilkrts" \
    "libgfortran" \
    "libgomp" \
    "libitm" \
    "libquadmath" \
    "libsanitizer/"{"a","l","ub","t"}"san" \
    "libstdc++-v3/src" \
    "libvtv"; do
    make \
      -C \
        "${CHOST}/${lib}" \
      DESTDIR="${pkgdir}" \
      install-toolexeclibLTLIBRARIES
  done
  make \
    -C \
      "${CHOST}/libmpx" \
    DESTDIR="${pkgdir}" \
    install
  rm \
    -f \
    "$pkgdir/${_libdir}/libmpx.spec"
  # Install Runtime Library Exception
  install \
    -vDm644 \
    "${srcdir}/${_pkg}/COPYING.RUNTIME" \
    "${pkgdir}/usr/share/licenses/${pkgbase}-libs/RUNTIME.LIBRARY.EXCEPTION"
}

package_gcc7() {
  local \
    _pkgdesc=()
  _pkgdesc=(
    "The GNU Compiler Collection"
    "- C and C++ frontends (7.x.x)"
  )
  pkgdesc="${_pkgdesc[*]}"
  depends=(
    "${pkgbase}-libs=${pkgver}-${pkgrel}"
    'binutils>=2.28'
    "libmpc"
  )
  echo "${depends[*]}"
  options+=(
    "staticlibs"
  )
  provides=(
    "${_pkg}=${pkgver}"
    "${_pkg}-static=${pkgver}"
    "${pkgbase}-static=${pkgver}"
  )
  export \
    LD_PRELOAD="/usr/lib/libstdc++.so"
  cd \
    "${_pkg}-build"
  make \
    -C \
      "${_pkg}" \
    DESTDIR="${pkgdir}" \
    install-driver \
    install-cpp \
    install-gcc-ar \
    c++.install-common \
    install-headers \
    install-plugin \
    install-lto-wrapper
  install \
    -vm755 \
    -t \
      "${pkgdir}/${_libdir}/" \
      "gcc/"{"cc1","cc1plus","collect2","lto1"}
  make \
    -C \
    "${CHOST}/lib${_pkg}" \
    DESTDIR="${pkgdir}" \
    install
  rm \
    -rf \
    "${pkgdir}/usr/lib/${_pkg}/${CHOST}/lib"*
  make \
    -C \
      "${CHOST}/libstdc++-v3/src" \
    DESTDIR="${pkgdir}" \
    install
  make \
    -C \
      "${CHOST}/libstdc++-v3/include" \
    DESTDIR="${pkgdir}" \
    install
  make \
    -C \
      "${CHOST}/libstdc++-v3/libsupc++" \
    DESTDIR="${pkgdir}" \
    install
  make \
    -C \
    "${CHOST}/libstdc++-v3/python" \
    DESTDIR="${pkgdir}" \
    install
  make \
    DESTDIR="${pkgdir}" \
    install-libcc1
  mv \
    "${pkgdir}/usr/lib/libcc1.so"* \
    "${pkgdir}/${_libdir}"
  rm \
    -f \
    "${pkgdir}/${_libdir}/libstdc++.so"*
  make \
    DESTDIR="${pkgdir}" \
    install-fixincludes
  make \
    -C \
      "${_pkg}" \
    DESTDIR="${pkgdir}" \
    install-mkheaders
  make \
    -C \
      "lto-plugin" \
    DESTDIR="${pkgdir}" \
    install
  make \
    -C \
      "${CHOST}/libcilkrts" \
    DESTDIR="${pkgdir}" \
    "install-nodist_"{"toolexeclib","cilkinclude"}"HEADERS"
  make \
    -C \
      "${CHOST}/libgomp" \
    DESTDIR="${pkgdir}" \
    "install-nodist_"{"libsubinclude","toolexeclib"}"HEADERS"
  make \
    -C \
      "${CHOST}/libitm" \
    DESTDIR="${pkgdir}" \
    "install-nodist_toolexeclibHEADERS"
  make \
    -C \
      "${CHOST}/libquadmath" \
    DESTDIR="${pkgdir}" \
    "install-nodist_libsubincludeHEADERS"
  make \
    -C \
      "${CHOST}/libsanitizer" \
    DESTDIR="${pkgdir}" \
    "install-nodist_"{"saninclude","toolexeclib"}"HEADERS"
  make \
    -C \
      "${CHOST}/libsanitizer/asan" \
    DESTDIR="${pkgdir}" \
    "install-nodist_toolexeclibHEADERS"
  make \
    -C \
      "${CHOST}/libmpx" \
    DESTDIR="${pkgdir}" \
    "install-nodist_toolexeclibHEADERS"
  make \
    -C \
      libcpp \
    DESTDIR="${pkgdir}" \
    install
  # many packages expect this symlink
  ln \
    -s \
    "${_pkg}-${_majver}" \
    "${pkgdir}/usr/bin/cc-${_majver}"
  rm \
    -f \
    "${pkgdir}/${_libdir}/lib"{"stdc++","${_pkg}_s"}".so"
  # byte-compile python libraries
  "${_py}" \
    -m \
      "compileall" \
    "${pkgdir}/usr/share/${_pkg}-${pkgver%%+*}/"
  python \
    -O \
    -m \
      "compileall" \
    "${pkgdir}/usr/share/${_pkg}-${pkgver%%+*}/"
  # Install Runtime Library Exception
  install \
    -vd \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
  ln \
    -s \
    "/usr/share/licenses/${pkgbase}-libs/RUNTIME.LIBRARY.EXCEPTION" \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
  # Remove conflicting files
  rm \
    -rf \
    "${pkgdir}/usr/share/locale"
}

package_gcc7-fortran() {
  local \
    _pkgdesc=()
  _pkgdesc=(
    "Fortran front-end"
    "for GCC (7.x.x)"
  )
  pkgdesc="${_pkgdesc[*]}"
  depends=(
    "${pkgbase}=${pkgver}-${pkgrel}"
  )
  echo "depends: ${depends[*]}"
  options=(
    '!emptydirs'
  )
  provides=(
    "${_pkg}-fortran=${pkgver}"
  )
  conflicts=(
    "${_pkg}-fortran=${pkgver}"
  )
  export \
    LD_PRELOAD="/usr/lib/libstdc++.so"
  cd \
    "${_pkg}-build"
  make \
    -C \
    "${CHOST}/libgfortran" \
    DESTDIR="${pkgdir}" \
    install-cafexeclibLTLIBRARIES \
    install-{"toolexeclibDATA","nodist_fincludeHEADERS"}
  make \
    -C \
    "${CHOST}/libgomp" \
    DESTDIR="${pkgdir}" \
    install-nodist_fincludeHEADERS
  make \
    -C \
      "${_pkg}" \
    DESTDIR="${pkgdir}" \
    fortran.install-common
  install \
    -vDm755 \
    "${_pkg}/f951" \
    "${pkgdir}/${_libdir}/f951"
  ln \
    -s \
    "gfortran-${_majver}" \
    "${pkgdir}/usr/bin/f95-${_pkgver}"
  # Install Runtime Library Exception
  install \
    -vd \
    "${pkgdir}/usr/share/licenses/${pkgname}"
  ln \
    -s \
    "../${_pkg}-libs/RUNTIME.LIBRARY.EXCEPTION" \
    "${pkgdir}/usr/share/licenses/${pkgname}/"
}

# vim:set sw=2 sts=-1 et:
