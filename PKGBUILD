pkgname=sshfs6
_pkgname=sshfs
pkgver=3.7.3
pkgrel=1
pkgdesc="FUSE client based on SFTP v6"
arch=('aarch64')
url="https://github.com/stevenxxiu/sshfs"
license=(GPL)
depends=(libfuse3 glib openssh)
makedepends=(meson python-docutils)
source=(git+https://github.com/stevenxxiu/sshfs.git)
sha256sums=('SKIP')
conflicts=('sshfs')

TERMUX_PREFIX='/data/data/com.termux/files/usr'

prepare() {
  cd $_pkgname

  yes | sudo pacman --config "${HOME}/pacman_aarch64.conf" --arch aarch64 --sync libfuse3 glib openssh
  sudo chown --recursive build $TERMUX_PREFIX

  export PATH="/opt/android-ndk/toolchains/llvm/prebuilt/linux-x86_64/bin:$PATH"
  export PKG_CONFIG="${startdir}/pkg-config"

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
