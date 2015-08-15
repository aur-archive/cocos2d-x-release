# Mantainer: daltomi@aol.com
# C++ Example for testing: http://www.wuala.com/daltomi/prog/HelloWorld.tar.xz

_major=3
_release=${_major}.2
_appname=cocos2d-x

pkgname=${_appname}-release
pkgver=${_release}
pkgrel=3
pkgdesc="Cocos2D-X is a game engine that supports multiple platforms such as iOS, Android, WinXP/7/8, WP8, BlackBerry, MeeGo, Marmelade, WebOS, Mac OS X"
arch=('i686' 'x86_64')
url="http://cdn.cocos2d-x.org/"
license=('MIT License')
depends=('glew' 'glfw' 'glibc' 'fontconfig' 'openssl' 'libgl' 'curl' 'libx11' 'fmodex' 'zlib' 'sqlite3' 'libpng' 'freetype2' 'libtiff')
makedepends=(cmake)
options=('!buildflags' 'staticlibs')
source=("${url}${_appname}-${_release}.zip"
        "cocos2dx.install"
        "cocos2dx.conf")
sha1sums=('3529479196123421ab9410997240da216ed5f86f'
          'e4866806369ea5c7789a0e99f66b63d15b4bcb04'
          '49292c52c5a3080721247b2e5b8a816e23db9b5b')
build() {
  _src_dir="${srcdir}/${_appname}-${_release}"
  cd ${_src_dir}/build
  cmake -DCMAKE_INSTALL_PREFIX:STRING=/usr -DDEBUG_MODE:BOOL=OFF -DCMAKE_BUILD_TYPE:STRING="Release" -DBUILD_CppTests:BOOL=OFF -DBUILD_LuaTests:BOOL=OFF -G "Unix Makefiles"  ..  && make
}

package() {
  _src_dir="${srcdir}/${_appname}-${_release}"
  _src_lib_dir=${_src_dir}/build/lib/
  _src_prebuild_dir=${_src_dir}/external
  _dest_include_dir=$pkgdir/usr/include/cocos2dx
  _dest_docs_dir=$pkgdir/usr/share/doc/cocos2dx
  _dest_licenses_dir=$pkgdir/usr/share/licenses/cocos2dx
  _dest_lib_dir=$pkgdir/usr/lib/cocos2dx

## Library
  install -d $pkgdir/usr/lib/cocos2dx
  cp -dp ${_src_lib_dir}* ${_dest_lib_dir}

## Pre-build Library
  if test "$CARCH" == x86_64; then
    cp -dp ${_src_prebuild_dir}/websockets/prebuilt/linux/64-bit/*.a ${_dest_lib_dir}
    cp -dp ${_src_prebuild_dir}/jpeg/prebuilt/linux/64-bit/*.a ${_dest_lib_dir}
#pacman provide -or- cp -dp ${_src_prebuild_dir}/linux-specific/fmod/prebuilt/64-bit/* ${_dest_lib_dir}
    cp -dp ${_src_prebuild_dir}/webp/prebuilt/linux/64-bit/*.a ${_dest_lib_dir}
#pacman provide -or- cp -dp ${_src_prebuild_dir}/freetype2/prebuilt/linux/64-bit/*.a ${_dest_lib_dir} 
#pacman provide -or- cp -dp ${_src_prebuild_dir}/tiff/prebuilt/linux/64-bit/*.a  ${_dest_lib_dir}
  else
    cp -dp ${_src_prebuild_dir}/websockets/prebuilt/linux/32-bit/*.a ${_dest_lib_dir}
    cp -dp ${_src_prebuild_dir}/jpeg/prebuilt/linux/32-bit/*.a ${_dest_lib_dir}
#pacman provide -or- cp -dp ${_src_prebuild_dir}/linux-specific/fmod/prebuilt/32-bit/* ${_dest_lib_dir}
    cp -dp ${_src_prebuild_dir}/webp/prebuilt/linux/32-bit/*.a ${_dest_lib_dir}
#pacman provide -or- cp -dp ${_src_prebuild_dir}/freetype2/prebuilt/linux/32-bit/*.a ${_dest_lib_dir} 
#pacman provide -or- cp -dp ${_src_prebuild_dir}/tiff/prebuilt/linux/32-bit/*.a  ${_dest_lib_dir}
  fi

## Config
  install -d $pkgdir/etc/ld.so.conf.d
  install -Dm 644 ${startdir}/cocos2dx.conf $pkgdir/etc/ld.so.conf.d/
  
## Headers
  install -d $_dest_include_dir
  install_recursive $_dest_include_dir cocos
  install_recursive $_dest_include_dir external
  install_recursive $_dest_include_dir extensions

## TODO:Docs, doxygen
 #install -d $_dest_docs_dir
 #install_recursive $_dest_docs_dir docs
 
## Licenses
  install -d $_dest_licenses_dir
  install_recursive $_dest_licenses_dir licenses
}

# Exclude all Windows, Mac, IOS, Android, Apple
install_recursive() {
  install -d $1/$2
  cd ${_src_dir}/$2
  _dir_cocos2dx=$(find . -type d -not -regex '.*\(winrt.*\|win32.*\|wp8.*\|apple.*\|proj.*\|mac.*\|ios.*\|android.*\)' )
  for dir in ${_dir_cocos2dx}
  do
    to_dir=$1/$2${dir/.}
    install -d "$to_dir"
    find ${dir} -maxdepth 1 \( -name "*.inl" -o -name "*.h" -o -name "*.lua" -o -name "*.txt" \) -exec install -Dm 644 "{}" "$to_dir" \; 
  done
}

# vim:set ts=2 sw=2 et:
