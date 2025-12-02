# Maintainer: Hani <itshanibee@gmail.com>
pkgname=kate-git
pkgver=26.03.70.r23303.g3cc127def4
pkgrel=1
pkgdesc="Advanced text editor built on the KDE Frameworks and Qt"
arch=('x86_64')
url="https://kate-editor.org/"
license=('LGPL-2.0-or-later' 'MIT')
depends=(
    'ktexteditor'
    'kconfig'
    'kcoreaddons'
    'kcrash'
    'kdbusaddons'
    'kguiaddons'
    'ki18n'
    'kiconthemes'
    'kio'
    'kparts'
    'kwindowsystem'
    'kxmlgui'
    'syntax-highlighting'
    'qt6-base'
    'qt6-wayland'
)
makedepends=(
    'cmake'
    'extra-cmake-modules'
    'git'
    'gettext'
    'kdoctools'
    'ninja'
    'qt6-tools'
)
optdepends=(
    'konsole: embedded terminal'
    'kuserfeedback: user feedback'
)
provides=('kate' 'kwrite')
conflicts=('kate' 'kwrite')

# Build from current directory
# Usage: Run makepkg from the kate source directory
source=()
sha256sums=()
noextract=()

pkgver() {
    cd "${startdir}"
    # Try to get version from CMakeLists.txt
    if [ -f CMakeLists.txt ]; then
        local major=$(grep -m1 "RELEASE_SERVICE_VERSION_MAJOR" CMakeLists.txt | sed 's/.*"\(.*\)".*/\1/')
        local minor=$(grep -m1 "RELEASE_SERVICE_VERSION_MINOR" CMakeLists.txt | sed 's/.*"\(.*\)".*/\1/')
        local micro=$(grep -m1 "RELEASE_SERVICE_VERSION_MICRO" CMakeLists.txt | sed 's/.*"\(.*\)".*/\1/')
        local gitcount=$(git rev-list --count HEAD 2>/dev/null || echo "0")
        local gitsha=$(git rev-parse --short HEAD 2>/dev/null || echo "unknown")
        echo "${major}.${minor}.${micro}.r${gitcount}.g${gitsha}"
    else
        echo "0.0.0.r0.g$(git rev-parse --short HEAD 2>/dev/null || echo unknown)"
    fi
}

prepare() {
    # Create symlink from srcdir to current directory
    if [ ! -e "${srcdir}/${pkgname%-git}" ]; then
        ln -sf "${startdir}" "${srcdir}/${pkgname%-git}"
    fi
    cd "${srcdir}/${pkgname%-git}"
    # Create build directory
    mkdir -p build
}

build() {
    cd "${srcdir}/${pkgname%-git}/build"
    
    cmake .. \
        -DCMAKE_BUILD_TYPE=Release \
        -DCMAKE_INSTALL_PREFIX=/usr \
        -DCMAKE_INSTALL_LIBDIR=lib \
        -DCMAKE_INSTALL_SYSCONFDIR=/etc \
        -DCMAKE_INSTALL_LOCALSTATEDIR=/var \
        -DQT_MAJOR_VERSION=6 \
        -DBUILD_TESTING=OFF \
        -Wno-dev
    
    cmake --build .
}

package() {
    cd "${srcdir}/${pkgname%-git}/build"
    DESTDIR="${pkgdir}" cmake --install .
}
