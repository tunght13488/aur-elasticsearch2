# Maintainer: Atte Lautanala <atte dot lautanala at gmail dot com>
# Contributor: Massimiliano Torromeo <massimiliano.torromeo@gmail.com>
# Contributor: Marcello "mererghost" Rocha <https://github.com/mereghost>
# Refactored by Blaž "Speed" Hrastnik <https://github.com/archSeer>

_pkgname=elasticsearch
pkgname=elasticsearch2
pkgver=2.4.6
pkgrel=1
pkgdesc="Distributed RESTful search engine built on top of Lucene"
arch=('any')
url="https://www.elastic.co/products/elasticsearch"
license=('APACHE')
depends=('java-runtime-headless' 'systemd')
provides=("elasticsearch=$pkgver")
install='elasticsearch2.install'
source=(
  "http://download.elasticsearch.org/$_pkgname/release/org/$_pkgname/distribution/tar/$_pkgname/$pkgver/$_pkgname-$pkgver.tar.gz"
  elasticsearch2.service
  elasticsearch2@.service
  elasticsearch2-sysctl.conf
  elasticsearch2-user.conf
  elasticsearch2-tmpfile.conf
  elasticsearch2.default
)
sha256sums=('5f7e4bb792917bb7ffc2a5f612dfec87416d54563f795d6a70637befef4cfc6f'
            '1b128a473938b57691cd78f9fd1c938fd0d8ed2f7d677592caaa1467d5151141'
            'd8559034c2372949c0dadff34d2f1cb65843ae3e8564aebaf37a00f77585a9f8'
            'b3feb1e9c7e7ce6b33cea6c727728ed700332aae942ca475c3bcc1d56b9f113c'
            '815f6a39db6f54bb40750c382ffbdc298d2c4c187ee8ea7e2f855923e2ff354b'
            'b797948fd11671058a7d3513567308b9ffc43f8c4162a7b098c01fa010c8f24c'
            'bb74e5fb8bc28f2125e015395ab05bea117b72bfc6dadbca827694b362ee0bf8')

backup=('etc/elasticsearch2/elasticsearch.yml'
        'etc/elasticsearch2/logging.yml'
        'etc/default/elasticsearch2')

prepare() {
  cd "$srcdir"/$_pkgname-$pkgver

  for script in plugin elasticsearch; do
    sed -e 's|^ES_HOME=.*dirname.*|ES_HOME=/usr/share/elasticsearch2|' \
        -e '/^ES_HOME=.*pwd/d' \
        -e 's|$ES_HOME/config|/etc/elasticsearch2|' \
        -i bin/$script
  done

  sed -re 's;#\s*(path\.conf:).*$;\1 /etc/elasticsearch2;' \
    -e '0,/#\s*(path\.data:).*$/s;;\1 /var/lib/elasticsearch2;' \
    -e 's;#\s*(path\.work:).*$;\1 /tmp/elasticsearch2;' \
    -e 's;#\s*(path\.logs:).*$;\1 /var/log/elasticsearch2;' \
    -i config/elasticsearch.yml
}

package() {
  cd "$pkgdir"
  install -dm750 etc/elasticsearch2/scripts
  install -dm755 usr/share/elasticsearch2/plugins
  install -dm755 var/lib/elasticsearch2
  install -dm755 var/log/elasticsearch2

  install -Dm644 "$srcdir"/elasticsearch2.service usr/lib/systemd/system/elasticsearch2.service
  install -Dm644 "$srcdir"/elasticsearch2@.service usr/lib/systemd/system/elasticsearch2@.service
  install -Dm644 "$srcdir"/elasticsearch2-user.conf usr/lib/sysusers.d/elasticsearch2.conf
  install -Dm644 "$srcdir"/elasticsearch2-tmpfile.conf usr/lib/tmpfiles.d/elasticsearch2.conf
  install -Dm644 "$srcdir"/elasticsearch2-sysctl.conf usr/lib/sysctl.d/elasticsearch2.conf
  install -Dm644 "$srcdir"/elasticsearch2.default etc/default/elasticsearch2

  cd "$srcdir"/$_pkgname-$pkgver
  cp -R lib modules "$pkgdir"/usr/share/elasticsearch2/
  cp config/* "$pkgdir"/etc/elasticsearch2/

  install -Dm755 bin/elasticsearch "$pkgdir"/usr/bin/elasticsearch2
  install -Dm755 bin/plugin "$pkgdir"/usr/bin/elasticsearch2-plugin
  install -Dm644 bin/elasticsearch.in.sh "$pkgdir"/usr/share/elasticsearch2/bin/elasticsearch.in.sh
}
