pkgname=pdftools
pkgver=0.7.1+d9be38f
pkgrel=1
pkgdesc='A collection of PDF command line tools'
arch=('any')
url='https://github.com/uroesch/pdftools'
license=('MIT')
depends=('pdftk')
makedepends=('git' 'make' 'fakeroot' 'asciidoctor-pdf' 'imagemagick')
provides=('img2pdf' 'ocrpdf' 'pdf2pdfa' 'pdfcat' 'pdfmeta' 'pdfresize' 'scan2jpg' 'scan2pdf' 'scan2png')
source=($pkgname::git+$url)
md5sums=('SKIP')

pkgver() {
  cd $pkgname
  _build=$(git rev-parse --short HEAD)
  _version=$(awk -F'[: ]' '($2 == "revnumber") {print $NF}' README.adoc)
  printf "%s+%s\n" $_version $_build
}

# # do not use: generates a lot of errors right now
#check() {
#  make test
#}

package() {
  cd $pkgname

  # build executables
  install -dm755 $pkgdir/usr/local/bin
  install -Dm755 ./bin/* $pkgdir/usr/local/bin/

  # create docs directory
  install -dm755 $pkgdir/usr/share/doc/$pkgname

  # put LICENSE file in docs directory
  install -Dm644 ./LICENSE $pkgdir/usr/share/doc/$pkgname/LICENSE

  # generate README in HTML and PDF formats
  make docs/README.html
  make docs/README.pdf

  # put generated README files in docs directory
  install -Dm644 ./docs/README.html $pkgdir/usr/share/doc/$pkgname/README.html
  install -Dm644 ./docs/README.pdf $pkgdir/usr/share/doc/$pkgname/README.pdf

  # build manpage
  install -dm755 ${pkgdir}/usr/share/man/man1
  asciidoctor -b manpage --out-file "$pkgdir/usr/share/man/man1/pdftools.1" README.adoc
}
