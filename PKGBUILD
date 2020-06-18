# -*- mode: shell-script;-*-
# Maintainer:  Johan Thorén <johan@thoren.xyz>
# Contributor: Vincent Grande <shoober420@gmail.com>
# Contributor: Static_Rocket
# Contributor: Jose Riha <jose1711 gmail com>
# Contributor: Sebastian J. Bronner <waschtl@sbronner.com>
# Contributor: Patrick Jackson <PatrickSJackson gmail com>
# Contributor: Christoph Vigano <mail@cvigano.de>

pkgname=st-jt
_pkgname=st
pkgver=0.8.3.r23.gb27a383
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X - Patched by Johan Thorén'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
provides=(st)
conflicts=(st)
url=https://st.suckless.org
source=(
    terminfo.patch
    README.terminfo.rst
    st-alpha.diff
    st-anysize.diff
    st-blinking_cursor.diff
    st-bold-is-not-bright.diff
    st-boxdraw_v2.diff
    st-clipboard.diff
    st-copyurl.diff
    st-desktopentry.diff
    st-disable-bold-italic-fonts.diff
    st-scrollback.diff
    st-scrollback-mouse.diff
    git://git.suckless.org/st)
sha256sums=(
    'f9deea445a5c6203a0e8e699f3c3b55e27275f17fb408562c4dd5d649edeea23'
    '0ebcbba881832adf9c98ce9fe7667c851d3cc3345077cb8ebe32702698665be2'
    'd6e38fc4b6bce35ef5e35d182f20ceb1c179192c6ffb8c7c6dc5ac5c0b7b6321'
    '156b41ef806bf3579452238729f698827c603768e1a231066a39cb2567aefc73'
    '4042801349a1ae0749a815636955c5ad14927a2cd2dec60ac691190c61a9b2b6'
    '077705116b78c53f9c99d81942ede103275d873b87ea3af2e36562935f46b880'
    '32e6a33cad20e11ff799dd8c06aa5ff92b1d8bffa6ec7329aa8b4e8be0cdb7d0'
    '0af564a96b90e5485029b6ebc2fd1c119c42577cbbc3cfa880e3c77b8a0c8a88'
    '7f161af7052301610afd4fb05c972d0d4fea6a9daf3cac1f6479638c9415393d'
    'f08a2f9bd62541af125a2e276a334451f2ae4519c0af658d4e2f949d0e724b49'
    '35272705301940b7b9c436f5170080db2b78dd147f579ba1c18cf9fc202ca20a'
    'e66575c735d715841bc1cbe16b1156dd2938f8005d1e1eee2650bce5b39ab4e0'
    'c3dbf454db422756af1face0d8cda6e20067969474647fe475ff89272e7934bf'
    'SKIP')
_gitname="st"
_sourcedir="$_gitname"
_makeopts="--directory=$_sourcedir"
_gitdir=${pkgname%'-git'}
builddir="$(pwd)"

pkgver() {
    cd $_pkgname
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

prepare() {
    cd $_pkgname

    echo $(pwd)

    echo "Adding terminfo patch:"
    patch --forward --strip=0 --input="${srcdir}/terminfo.patch"
    echo ""

    echo "Adding alpha patch:"
    patch --forward --strip=1 --input="${srcdir}/st-alpha.diff"
    echo ""

    echo "Adding anysize patch:"
    patch --forward --strip=1 --input="${srcdir}/st-anysize.diff"
    echo ""

    echo "Adding blinking_cursor patch:"
    patch --forward --strip=1 --input="${srcdir}/st-blinking_cursor.diff"
    echo ""

    echo "Adding bold-is-not-bright patch:"
    patch --forward --strip=1 --input="${srcdir}/st-bold-is-not-bright.diff"
    echo ""

    echo "Adding boxdraw_v2 patch:"
    patch --forward --strip=1 --input="${srcdir}/st-boxdraw_v2.diff"
    echo ""

    echo "Adding clipboard patch:"
    patch --forward --strip=1 --input="${srcdir}/st-clipboard.diff"
    echo ""

    echo "Adding copyurl patch:"
    patch --forward --strip=1 --input="${srcdir}/st-copyurl.diff"
    echo ""

    echo "Adding desktopentry patch:"
    patch --forward --strip=1 --input="${srcdir}/st-desktopentry.diff"
    echo ""

    echo "Adding disable-bold-italic-fonts patch:"
    patch --forward --strip=1 --input="${srcdir}/st-disable-bold-italic-fonts.diff"
    echo ""

    echo "Adding scrollback patch:"
    patch --forward --strip=1 --input="${srcdir}/st-scrollback.diff"
    echo ""

    echo "Adding scrollback-mouse patch:"
    patch --forward --strip=1 --input="${srcdir}/st-scrollback-mouse.diff"
    echo ""

    # echo "Adding patch personal_config:"
    # patch --forward --strip=1 --input="${srcdir}/personal_config.diff"
    # echo ""

    # This package provides a mechanism to provide a custom config.h. Multiple
    # configuration states are determined by the presence of two files in
    # $_pkgname:
    #
    # config.h  config.def.h  state
    # ========  ============  =====
    # absent    absent        Initial state. The user receives a message on how
    #                         to configure this package.
    # absent    present       The user was previously made aware of the
    #                         configuration options and has not made any
    #                         configuration changes. The package is built using
    #                         default values.
    # present                 The user has supplied his or her configuration. The
    #                         file will be copied to $srcdir and used during
    #                         build.
    #
    # After this test, config.def.h is copied from $srcdir/src/st to $srcdir to
    # provide an up to date template for the user.
    if [ -e "${builddir}/config.h" ]
    then
        cp "${builddir}/config.h" "${srcdir}/st/config.h"
    elif [ ! -e "${builddir}/config.def.h" ]
    then
        msg='This package can be configured in config.h. Move the config.def.h '
        msg+='that was just placed into the package directory to config.h and '
        msg+='modify it to change the configuration. Or just leave it alone to '
        msg+='continue to use default values.'
        warning "$msg"
    fi
    cp "${srcdir}/st/config.def.h" "${builddir}/config.def.h"
}

package() {
  local installopts='--mode 0644 -D --target-directory'
  local shrdir="$pkgdir/usr/share"
  local licdir="$shrdir/licenses/$pkgname"
  local docdir="$shrdir/doc/$pkgname"
  make $_makeopts PREFIX=/usr DESTDIR="$pkgdir" install
  install $installopts "$licdir" "$_sourcedir/LICENSE"
  install $installopts "$docdir" "$_sourcedir/README"
  install $installopts "$docdir" README.terminfo.rst
  install $installopts "$shrdir/$pkgname" "$_sourcedir/st.info"
}
