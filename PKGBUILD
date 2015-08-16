# custom variables
_hkgname=glade
_licensefile=COPYING

# PKGBUILD options/directives
pkgname=haskell-glade
pkgver=0.12.1
pkgrel=4
pkgdesc="Binding to the glade library."
url="http://projects.haskell.org/gtk2hs/"
license=("LGPL-2.1")
arch=('i686' 'x86_64')
makedepends=()
depends=("ghc"
         "gtk2hs-buildtools"
         "haskell-glib=0.12.3.1"
         "haskell-gtk=0.12.3.1"
	 "libglade")
options=('strip')
source=("http://hackage.haskell.org/packages/archive/${_hkgname}/${pkgver}/${_hkgname}-${pkgver}.tar.gz")
install="${pkgname}.install"
md5sums=('26cec14037feb9f9cac1c2d293caf2a8')

# PKGBUILD functions
build() {
    cd ${srcdir}/${_hkgname}-${pkgver}

    runhaskell Setup configure -O -p --enable-split-objs --enable-shared \
        --prefix=/usr --docdir=/usr/share/doc/${pkgname} \
        --libsubdir=\$compiler/site-local/\$pkgid
    runhaskell Setup build
    runhaskell Setup haddock
    runhaskell Setup register --gen-script
    runhaskell Setup unregister --gen-script
    sed -i -r -e "s|ghc-pkg.*unregister[^ ]* |&'--force' |" unregister.sh
}

package() {
    cd ${srcdir}/${_hkgname}-${pkgver}
    install -D -m744 register.sh   ${pkgdir}/usr/share/haskell/${pkgname}/register.sh
    install    -m744 unregister.sh ${pkgdir}/usr/share/haskell/${pkgname}/unregister.sh
    install -d -m755 ${pkgdir}/usr/share/doc/ghc/html/libraries
    ln -s /usr/share/doc/${pkgname}/html ${pkgdir}/usr/share/doc/ghc/html/libraries/${_hkgname}
    runhaskell Setup copy --destdir=${pkgdir}
    install -D -m644 ${_licensefile} ${pkgdir}/usr/share/licenses/${pkgname}/LICENSE
    rm -f ${pkgdir}/usr/share/doc/${pkgname}/${_licensefile}
}
