# Maintainer: Florian Bruhin (The Compiler) <archlinux.org@the-compiler.org>
# Adapted for dp1 version by Semyon Maryasin <simeon@maryasin.name>
# vim: ft=sh

pkgname=pebble-sdk-beta
pkgver=3.0_dp6
pkgrel=1
pkgdesc="Pebble SDK, used to develop applications and watchfaces for the Pebble Smartwatch."
url="https://developer.getpebble.com/2/getting-started/"
arch=('i386' 'x86_64')
license=('custom' 'MIT')
install='pebble-sdk.install'
depends=('arm-none-eabi-gcc' 'arm-none-eabi-newlib' 'python2-pypng' 'python2-sh' 'python2-freetype-py'
         'python2-sh' 'python2-twisted' 'python2-autobahn'
         'python2-websocket-client' 'arm-none-eabi-binutils'
         'python2-greenlet' 'python2-requests'
         'python2-six' 'python2-wsgiref'
         'python2-gevent' 'python2-gevent-websocket' 'python2-pygeoip' 'qemu-fdt')
#         'pypy-backports.ssl_match_hostname'
optdepends=('python2-pyserial: To connect to the Pebble via serial port')
conflicts=('pebble-sdk')
source=("http://assets.getpebble.com.s3-website-us-east-1.amazonaws.com/sdk2/PebbleSDK-${pkgver/_/-}.tar.gz"
        'python-waf.patch'
        'build-command.patch'
        'pebble-sdk.install')
sha1sums=('568ec2f553bc7e577c19400979c2fd26b4a5f55f'
          'a59d059fb75db7cfdebb008917a2b96459bdaa38'
          '72be6cd5697dc401f89544578aa4c4f8a97905f9'
          '7ea5244f828e682d073434078569fab62a1ad996')
options=('staticlibs' '!strip')

prepare() {
  cd "$srcdir/PebbleSDK-${pkgver//_/-}"
  find . -type f \( -path ./bin/pebble -o -path ./Pebble/waf -o \
                    -name '*.py' \) -exec \
    sed -i '1s|^#!/usr/bin/env python$|#!/usr/bin/python2|' {} \;
  patch -p0 -i "$srcdir/build-command.patch"
  # Unpack waf and fix python files
  cd Pebble
  ./waf 2>/dev/null || true
  cd .waf*
  find . -type f -name '*.py' -exec \
    sed -i '1s|^#! /usr/bin/env python$|#!/usr/bin/python2|' {} \;
  patch -p0 -i "$srcdir/python-waf.patch"
}

package() {
  install -dm755 "$pkgdir/opt/pebble"
  install -dm755 "$pkgdir/usr/bin"
  cd "$srcdir/PebbleSDK-${pkgver//_/-}"
  cp -R * "$pkgdir/opt/pebble"
  rm -r "$pkgdir/opt/pebble/bin"
  ln -s "/opt/pebble/tools/pebble.py" "$pkgdir/usr/bin/pebble"
  touch "$pkgdir/opt/pebble/NO_TRACKING"
}

# vim:set ts=2 sw=2 et:
