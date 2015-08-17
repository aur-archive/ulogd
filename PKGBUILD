# Maintainer: Sebastien Luttringer <seblu+arch@seblu.net>
# Contributor: Dale Blount <dale@archlinux.org>
# Contributor: Sergej Pupykin <ps@lx-ltd.ru>
pkgname=ulogd
pkgver=1.24
pkgrel=6
pkgdesc='Userspace Packet Logging for netfilter'
arch=('i686' 'x86_64')
url='http://www.netfilter.org/projects/ulogd/index.html'
license=('GPL2')
makedepends=('mysql' 'postgresql' 'sqlite3' 'libpcap')
backup=('etc/ulogd.conf')
source=(
  "ftp://ftp.netfilter.org/pub/$pkgname/$pkgname-$pkgver.tar.bz2"
  'rc'
  'logrotate'
)
md5sums=('05b4ed2926b9a22aaeaf642917bbf8ff'
         '99fd661b5e689a25e0d6323fb3591d9e'
         'fe40b3073b7474a77e0b8b0bfd19ab63')

build() {
  cd $pkgname-$pkgver
  export MAKEFLAGS="-j1"
  ./configure --prefix=/usr --sysconfdir=/etc --with-mysql --with-pgsql --with-sqlite3
  make || true
  (cd mysql && ld -shared -L/usr/lib -L/usr/lib -L/usr/lib/mysql -lmysqlclient -lz -lcrypt -lnsl -lm -o ulogd_MYSQL.so ulogd_MYSQL_sh.o -lc)
  make
}

package() {
  cd $pkgname-$pkgver
  make DESTDIR="$pkgdir" install
  chmod 644 "$pkgdir/etc/ulogd.conf"
  install -d -m 755 "$pkgdir/usr/share/ulogd"
  install -m644 doc/*.table "$pkgdir/usr/share/ulogd"
  install -D -m644 "$srcdir/logrotate" "$pkgdir/etc/logrotate.d/ulogd"
  install -D -m755 "$srcdir/rc" "$pkgdir/etc/rc.d/ulogd"
}

# vim:set ts=2 sw=2 ft=sh et:
