pkgname=(
    qt6-qtbase
    qt6-xcb-private-headers
)
pkgbase=qt6-qtbase
pkgver=6.9.2
pkgrel=2
pkgdesc="A cross-platform application and UI framework"
arch=('x86_64')
url="https://www.qt.io"
license=(
    'GPL-3.0-only'
    'LGPL-3.0-only'
    'LicenseRef-Qt-Commercial'
    'Qt-GPL-exception-1.0'
)
depends=(
    'brotli'
    'dbus'
    'double-conversion'
    'fontconfig'
    'freetype2'
    'gcc-libs'
    'glib2'
    'glibc'
    'harfbuzz'
    'icu'
    'krb5'
    'libb2'
    'libcups'
    'libdrm'
    'libglvnd'
    'libice'
    'libinput'
    'libjpeg-turbo'
    'libpng'
    'libproxy'
    'libsm'
    'libx11'
    'libxcb'
    'libxkbcommon'
    'libxkbcommon-x11'
    'md4c'
    'mesa'
    'mtdev'
    'openssl'
    'pcre2'
    'shared-mime-info'
    'sqlite'
    'systemd-libs'
    'tslib'
    'xcb-util-cursor'
    'xcb-util-image'
    'xcb-util-keysyms'
    'xcb-util-renderutil'
    'xcb-util-wm'
    'xdg-utils'
    'zlib'
    'zstd'
)
makedepends=(
    'alsa-lib'
    'cmake'
    'cups'
    'freetds'
    'gst-plugins-base-libs'
    'gtk3'
    'libfbclient'
    'libpulse'
    'mariadb-libs'
    'ninja'
    'postgresql'
    'unixodbc'
    'vulkan-headers'
    'xmlstarlet'
)
source=(https://download.qt.io/archive/qt/${pkgver%.*}/${pkgver}/submodules/${pkgbase#*-}-everywhere-src-${pkgver}.tar.xz
    qt6-qtbase-cflags.patch
    qt6-qtbase-nostrip.patch)
sha256sums=(44be9c9ecfe04129c4dea0a7e1b36ad476c9cc07c292016ac98e7b41514f2440
    5411edbe215c24b30448fac69bd0ba7c882f545e8cf05027b2b6e2227abc5e78
    4b93f6a79039e676a56f9d6990a324a64a36f143916065973ded89adc621e094)

prepare() {
    cd ${pkgbase#*-}-everywhere-src-${pkgver}

    patch -p1 < ${srcdir}/qt6-qtbase-cflags.patch
    patch -p1 < ${srcdir}/qt6-qtbase-nostrip.patch
}

build() {
    cd ${pkgbase#*-}-everywhere-src-${pkgver}

    local cmake_args=(
        -B flarebird-build
        -G Ninja
        -D CMAKE_BUILD_TYPE=RelWithDebInfo
        -D CMAKE_INSTALL_PREFIX=/usr
        -D INSTALL_LIBDIR=lib64
        -D INSTALL_BINDIR=lib64/qt6/bin
        -D INSTALL_PUBLICBINDIR=usr/bin
        -D INSTALL_DOCDIR=share/doc/qt6
        -D INSTALL_ARCHDATADIR=lib64/qt6
        -D INSTALL_DATADIR=share/qt6
        -D INSTALL_INCLUDEDIR=include/qt6
        -D INSTALL_MKSPECSDIR=lib64/qt6/mkspecs
        -D INSTALL_EXAMPLESDIR=share/doc/qt6/examples
        -D FEATURE_journald=ON
        -D FEATURE_libproxy=ON
        -D FEATURE_openssl_linked=ON
        -D FEATURE_system_sqlite=ON
        -D FEATURE_system_xcb_xinput=ON
        -D FEATURE_no_direct_extern_access=ON
        -D CMAKE_INTERPROCEDURAL_OPTIMIZATION=ON
        -D CMAKE_MESSAGE_LOG_LEVEL=STATUS
    )

    cmake "${cmake_args[@]}"

    cmake --build flarebird-build
}

package_qt6-qtbase() {
    pkgdesc="A cross-platform application and UI framework"
    depends+=('qt6-qttranslations')

    cd ${pkgbase#*-}-everywhere-src-${pkgver}

    DESTDIR=${pkgdir} cmake --install flarebird-build

    install -vdm755 ${pkgdir}/usr/bin
    for i in $(ls ${pkgdir}/usr/lib64/qt6/bin); do
        ln -s /usr/lib64/qt6/bin/${i} ${pkgdir}/usr/bin/${i}
    done
}

package_qt6-xcb-private-headers() {
    pkgdesc="Private headers for Qt6 Xcb"
    depends=("qt6-qtbase=${pkgver}")

    cd ${pkgbase#*-}-everywhere-src-${pkgver}

    install -d -m755 ${pkgdir}/usr/include/qt6xcb-private/{gl_integrations,nativepainting}
    cp -r src/plugins/platforms/xcb/*.h ${pkgdir}/usr/include/qt6xcb-private/
    cp -r src/plugins/platforms/xcb/gl_integrations/*.h ${pkgdir}/usr/include/qt6xcb-private/gl_integrations/
    cp -r src/plugins/platforms/xcb/nativepainting/*.h ${pkgdir}/usr/include/qt6xcb-private/nativepainting/
}
