pkgname=(
    qt6-qtbase
    qt6-xcb-private-headers
)
pkgbase=qt6-qtbase
pkgver=6.10.0
pkgrel=3
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
    'git'
    'gst-plugins-base-libs'
    'gtk3'
    'jemalloc'
    'libfbclient'
    'libpulse'
    'mariadb-libs'
    'ninja'
    'postgresql'
    'unixodbc'
    'vulkan-headers'
    'xmlstarlet'
)
source=(git+https://code.qt.io/qt/${pkgname#*-}#tag=v${pkgver}
    qt6-qtbase-cflags.patch
    qt6-qtbase-nostrip.patch)
sha256sums=(be60bf981a67824c2a27155d794eb30d10a6daa71db37695668a8a703acff929
    5411edbe215c24b30448fac69bd0ba7c882f545e8cf05027b2b6e2227abc5e78
    4b93f6a79039e676a56f9d6990a324a64a36f143916065973ded89adc621e094)

prepare() {
    cd ${pkgbase#*-}

    patch -p1 < ${srcdir}/qt6-qtbase-cflags.patch
    patch -p1 < ${srcdir}/qt6-qtbase-nostrip.patch

    # Fix yakuake
    git cherry-pick -n a374ab6ce9f01f1f559403ec377cde990a689890
}

build() {
    cd ${pkgbase#*-}

    local cmake_args=(
        -B flarebird-build
        -G Ninja
        -D CMAKE_BUILD_TYPE=Release
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

    cd ${pkgbase#*-}

    DESTDIR=${pkgdir} cmake --install flarebird-build

    install -vdm755 ${pkgdir}/usr/bin
    for i in $(ls ${pkgdir}/usr/lib64/qt6/bin); do
        ln -s /usr/lib64/qt6/bin/${i} ${pkgdir}/usr/bin/${i}
    done
}

package_qt6-xcb-private-headers() {
    pkgdesc="Private headers for Qt6 Xcb"
    depends=("qt6-qtbase=${pkgver}")

    cd ${pkgbase#*-}

    install -d -m755 ${pkgdir}/usr/include/qt6xcb-private/{gl_integrations,nativepainting}
    cp -r src/plugins/platforms/xcb/*.h ${pkgdir}/usr/include/qt6xcb-private/
    cp -r src/plugins/platforms/xcb/gl_integrations/*.h ${pkgdir}/usr/include/qt6xcb-private/gl_integrations/
    cp -r src/plugins/platforms/xcb/nativepainting/*.h ${pkgdir}/usr/include/qt6xcb-private/nativepainting/
}
