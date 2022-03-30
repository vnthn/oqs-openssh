# Maintainer: Nils von Nethen <nils.nethen@stud.h-da.de>
pkgname=oqs-openssh
pkgver=OQS.OpenSSH.snapshot.2022.01.r6.ga3fa262b
pkgrel=1
pkgdesc="Open Quantum Safe OpenSSH (NOT FOR PRODUCTIVE USE)"
arch=('any')
url="https://github.com/open-quantum-safe/openssh"
license=('MIT')
groups=()
depends=()
makedepends=('cmake' 'gcc' 'ninja' 'openssl' 'python-pytest' 'python-pytest-xdist' 'unzip' 'libxslt' 'doxygen' 'graphviz' 'liboqs' 'autoconf' 'automake' 'libtool' 'zlib')
provides=("${pkgname%-git}")
conflicts=("${pkgname%-git}" 'openssh')
replaces=('openssh')
backup=()
options=()
install="${pkgname}.install"
source=('git+https://github.com/open-quantum-safe/openssh.git' 'sshd.service' 'ssh_config' 'sshd_config')
noextract=()
md5sums=('SKIP' 'SKIP' 'SKIP' 'SKIP')

# Please refer to the 'USING VCS SOURCES' section of the PKGBUILD man page for
# a description of each element in the source array.

pkgver() {
    cd "$srcdir/openssh"
    git describe --long --tags | sed 's/^v//;s/\([^-]*-g\)/r\1/;s/-/./g'
}

build() {
    cd "$srcdir/openssh"
    echo "$pkgdir"
    set -exo pipefail

    #INSTALL_PREFIX=${INSTALL_PREFIX:-"`pwd`/oqs-test/tmp"}
    WITH_OPENSSL=${WITH_OPENSSL:-"true"}
    SYSCONFDIR="/etc/ssh"
    INSTALL_PREFIX="$pkgdir/usr"

    
    case "$OSTYPE" in
        darwin*)  OPENSSL_SYS_DIR=${OPENSSL_SYS_DIR:-"/usr/local/opt/openssl@1.1"} ;;
        linux*)   OPENSSL_SYS_DIR=${OPENSSL_SYS_DIR:-"/usr"} ;;
        *)        echo "Unknown operating system: $OSTYPE" ; exit 1 ;;
    esac
    
    if [ -f Makefile ]; then
        make clean
    else
        autoreconf -i
    fi
    
    if [ "x${WITH_OPENSSL}" == "xtrue" ]; then
        #./configure --prefix="${INSTALL_PREFIX}" --with-ldflags="-Wl,-rpath -Wl,${INSTALL_PREFIX}/lib" --with-libs=-lm --with-ssl-dir="${OPENSSL_SYS_DIR}" --with-liboqs-dir="`pwd`/oqs" --with-cflags="-Wno-implicit-function-declaration -I${INSTALL_PREFIX}/include" --sysconfdir="${SYSCONFDIR}"
        ./configure --with-ldflags="-Wl,-rpath -Wl,${INSTALL_PREFIX}/lib" --with-libs=-lm --with-ssl-dir="${OPENSSL_SYS_DIR}" --with-liboqs-dir="`pwd`/oqs" --with-cflags="-Wno-implicit-function-declaration -I${INSTALL_PREFIX}/include" --sysconfdir="${SYSCONFDIR}" --prefix="/usr" --sbindir="/usr/bin" --libexecdir="/usr/lib/ssh"
    else
        #./configure --prefix="${INSTALL_PREFIX}" --with-ldflags="-Wl,-rpath -Wl,${INSTALL_PREFIX}/lib" --with-libs=-lm --without-openssl --with-liboqs-dir="`pwd`/oqs" --with-cflags="-I${INSTALL_PREFIX}/include" --sysconfdir="${SYSCONFDIR}"
        ./configure --with-ldflags="-Wl,-rpath -Wl,${INSTALL_PREFIX}/lib" --with-libs=-lm --without-openssl --with-liboqs-dir="`pwd`/oqs" --with-cflags="-I${INSTALL_PREFIX}/include" --sysconfdir="${SYSCONFDIR}" --prefix="/usr" --sbindir="/usr/bin" --libexecdir="/usr/lib/ssh"
    fi
    if [ "x${CIRCLECI}" == "xtrue" ] || [ "x${TRAVIS}" == "xtrue" ]; then
        make -j2
    else
        make -j
    fi
    autoreconf -i
    make
}

package() {
    install -m755 -d ${pkgdir}/etc/ssh || return 1
    install -m644 $startdir/ssh_config ${pkgdir}/etc/ssh/ssh_config || return 1
    install -m644 $startdir/sshd_config ${pkgdir}/etc/ssh/sshd_config || return 1
    install -m755 -d ${pkgdir}/usr/lib/systemd/system || return 1
    install -m644 $startdir/sshd.service ${pkgdir}/usr/lib/systemd/system/sshd.service || return 1
    cd "$srcdir/openssh"
    mkdir -p "$pkgdir/usr/bin"
    make DESTDIR="$pkgdir/" install
}
