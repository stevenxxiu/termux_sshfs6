pkgname=sshfs6
_pkgname=sshfs
pkgver=3.7.3
pkgrel=1
pkgdesc="FUSE client based on SFTP v6"
arch=('aarch64')
url="https://github.com/stevenxxiu/sshfs"
license=(GPL)
depends=(libfuse3 glib openssh)
source=(git+https://github.com/stevenxxiu/sshfs.git)
sha256sums=('SKIP')
conflicts=('sshfs')

TERMUX_PREFIX='/data/data/com.termux/files/usr'

prepare() {
  doas apk add meson py3-docutils

  cd $_pkgname

  yes | doas pacman --config "${HOME}/pacman_aarch64.conf" --arch aarch64 --sync libfuse3 glib openssh
  doas chown --recursive build $TERMUX_PREFIX

  export PATH="$startdir:$PATH"

  patch --silent --strip 1 --input "${startdir}/line-max.patch"

  rm -rf build
  mkdir build
  cd build

  CC=gcc CXX=g++ CFLAGS= CXXFLAGS= CPPFLAGS= LDFLAGS= meson setup \
    --cross-file "${startdir}/meson-crossfile.txt" \
    --prefix $TERMUX_PREFIX \
    --libdir lib \
    --buildtype minsize \
    --strip \
    ..
}

build() {
  cd $_pkgname/build
  ninja
}

package() {
  cd $_pkgname/build

  DESTDIR="${pkgdir}" ninja install
}
