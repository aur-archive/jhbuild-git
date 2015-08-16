# Maintainer: Techlive Zheng <techlivezheng@gmail.com>
# Contributor: Thijs Vermeir <thijsvermeir@gmail.com>

pkgname=jhbuild-git
_pkgname='jhbuild'
pkgver=3.5.91.r1021.gaa6462d
pkgrel=1
pkgdesc='\
JHBuild allows you to automatically download and compile Gnome\
 "modules" (i.e. source code packages).'
arch=('i686' 'x86_64')
license=('GPL')
provides=('jhbuild')
conflicts=('jhbuild')
depends=(
    python2
    git
    cvs
    rsync
    subversion
    ruby
    ragel
    bluez
    boost
    gperf
    gypsy
    espeak
    doxygen
    highlight
    libgphoto2
    bogofilter
    docbook-xsl
    spamassassin
    gnome-doc-utils
    xf86-input-wacom
    xorg-util-macros
    intltool
    yelp-tools
    gnome-common
    xtrans
)
optdepends=(
)
makedepends=(
    git
    intltool
    yelp-tools
    gnome-common
)
install=jhbuild-git.install
source=(
    git://git.gnome.org/jhbuild
    use_python2_when_building_modules.patch
)
md5sums=(
    'SKIP'
    '513d174321c3d772423502e00b3034cf'
)
url='https://live.gnome.org/Jhbuild/'

pkgver() {
    cd "${_pkgname}"
    git describe --always | sed -E 's/([^-]*-g)/r\1/;s/-/./g'
}

build() {
    cd "${_pkgname}"

    patch -p1 < ../use_python2_when_building_modules.patch

    ./autogen.sh --prefix=/usr PYTHON=/usr/bin/python2

    make
}

package() {
    cd "${_pkgname}"

    make DESTDIR="${pkgdir}" install

    # use python2 for jhbuild
    sed -i '1 s/python/python2/' "${pkgdir}/usr/bin/jhbuild"

    # keep the source, jhbuild needs them
    install -d "${pkgdir}/usr/share/jhbuild/src"
    git archive --format=tar HEAD | (cd "${pkgdir}/usr/share/jhbuild/src" && tar xf -)
    sed -i "s:$(pwd):/usr/share/jhbuild/src:g" "${pkgdir}/usr/bin/jhbuild"
}
