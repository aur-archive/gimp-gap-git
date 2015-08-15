# Maintainer:
# Contributor: Alexander Rødseth <rodseth@gmail.com>
# Contributor: tobias <tobias@archlinux.org>
# Contributor: Tobias Kieslich <tobias@justdreams.de>

pkgname=gimp-gap-git
pkgver=2.6.0
pkgrel=1
pkgdesc='Plugins for editing and creating animations with The Gimp'
arch=('x86_64' 'i686')
license=('GPL')
url='https://github.com/GNOME/gimp-gap'
depends=('gimp' 'xvidcore' 'bash' 'libjpeg')
makedepends=('intltool' 'setconf')
#source=("ftp://ftp.gimp.org/pub/gimp/plug-ins/v2.6/gap/$pkgname-$pkgver.tar.bz2"
source=("$pkgname.tgz::https://github.com/GNOME/gimp-gap/archive/master.tar.gz")
sha256sums=('5deb2fa72b465f5e15f83b6175e1635c9e2ae0d3561c21260a549dbaae10c3eb')

build() {
  cd "$srcdir/gimp-gap-master"

  sed -i 's:1.7:1.13:' autogen.sh
  sed -i 's:AM_CONFIG_HEADER:AC_CONFIG_HEADERS:' configure.in
  sed -i 's:AM_PROG_CC_STDC:AC_PROG_CC:' configure.in
  ./autogen.sh

  # -lm must be one of the last libraries for the compilation to succeed
  sed -i 's/-lm"/"/' configure

  # libmpeg3 is not in [extra], only libmpeg2
  # gimp-gap comes with its own version of ffmpeg and libmpeg3
  ./configure \
    --prefix=/usr \
    --exec-prefix=/usr \
    --sbindir=/usr/bin \
    --sysconfdir=/etc \
    --localstatedir=/var \
    --libdir=/usr/lib \
    --includedir=/usr/include \
    --datadir=/usr/share \
    --disable-libavformat \
    --disable-libmpeg3

  cd extern_libs/libmpeg3
  ./configure
  cd ../ffmpeg
  ./configure
  cd ../..

  # Add -lm as the last library to be linked and start compiling
  make LIBS+=' -lm'
}

package() {
  cd "$srcdir/gimp-gap-master"
  
  make DESTDIR="$pkgdir" install
}

# vim:set ts=2 sw=2 et:
