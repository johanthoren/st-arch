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
pkgver=0.8.4.r3.g9e68fdb
pkgrel=1
pkgdesc='A simple virtual terminal emulator for X - Patched by Johan Thorén'
arch=('i686' 'x86_64')
license=('MIT')
depends=('libxft')
makedepends=('ncurses' 'libxext' 'git')
optdepends=('ttf-joypixels'
            'ttf-nerdfonts-hack-complete-git'
            'otf-inconsolata-powerline-git'
            'dmenu')
provides=(st)
conflicts=(st)
url=https://st.suckless.org
source=(
    README.terminfo.rst
    jt-patches.diff
    git://git.suckless.org/st)
sha256sums=(
    '0ebcbba881832adf9c98ce9fe7667c851d3cc3345077cb8ebe32702698665be2'
    '47b246421ca93129210ee972de63f8416a8253592cd85fbd9fffd62539ff51c4'
    'SKIP'
)
_gitname="st"
_sourcedir="$_gitname"
_makeopts="--directory=$_sourcedir"
_gitdir=${pkgname%'-git'}
builddir="$(pwd)"

pkgver() {
    cd $_pkgname
    git describe --long --tags | sed 's/\([^-]*-g\)/r\1/;s/-/./g'
}

_patch_it() {
    echo "Adding patch $1"
    patch --forward --strip=1 --input="${srcdir}/${1}"
    echo ""
}

prepare() {
    cd $_pkgname

    patches=(
        jt-patches.diff
            )

    for p in "${patches[@]}"; do
        _patch_it "$p"
    done


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
