# Contributor: graysky <graysky AT archlinux dot us>

pkgname=monitorix-beta
_pkgname=monitorix
pkgver=3.0.0
pkgrel=2
pkgdesc='A lightweight system monitoring tool that uses rrd databases. Beta 3.x branch.'
arch=('any')
license=('GPL')
url="http://www.$_pkgname.org"
depends=('perl' 'perl-cgi' 'perl-mailtools' 'perl-mime-lite' 'perl-libwww' 'perl-dbi' 'perl-xml-simple' 'perl-config-simple' 'perl-config-general' 'rrdtool')
optdepends=('anything-sync-daemon: offload your databases to tmpfs to save i/o to your disk.'
'lm_sensors: enable support for system temp monitoring.'
'smartmontools: enable support for hdd bad sector monitoring.'
'nvidia: enable support for nVidia card temp and usage monitoring.'
'hddtemp: enable support for hdd temp monitoring.'
'terminus-font: if your graphs do not contain letters and numbers you may need this font package.')
provides=$_pkgname
conflicts=$_pkgname
backup=etc/$_pkgname.conf
source=(http://www.$_pkgname.org/$_pkgname-$pkgver.tar.gz rc.$_pkgname $_pkgname.service)
sha256sums=('b39292690b35189811a348f165886c98d598f183251ca4ec62ae01f5660f8703'
            'b9471a8fbe808a5fac091f257585d82cd67b701a3c4ee55b7a4f9e9c8579854f'
            '534270c9c0355df5e9f86a46217f6a2f475c18643697be8ad2b6a1ebede2564f')
install=readme.install
_basedir="srv/http/monitorix"
_libdir="var/lib/$_pkgname"

package() {
	cd "$srcdir"/$_pkgname-$pkgver
	install -Dm755 "$srcdir"/$_pkgname.service "$pkgdir"/usr/lib/systemd/system/$_pkgname.service

	# use Arch defaults in config
	sed -i -e '/^base_dir/ s,\/usr\/share,\/srv\/http,' -i -e '/^base_cgi/ s,-cgi,\/cgi-bin,' $_pkgname.conf
	install -Dm755 $_pkgname "$pkgdir"/usr/bin/$_pkgname
	install -Dm755 $_pkgname.cgi "$pkgdir"/$_basedir/cgi-bin/$_pkgname.cgi
	install -Dm644 $_pkgname.conf "$pkgdir"/etc/$_pkgname.conf
	
	for i in lib/*.pm; do
		install -Dm644 "$i" "$pkgdir/usr/lib/$_pkgname/${i##lib/}"
	done

	for i in Changes COPYING README*; do
		install -Dm644 "$i" "$pkgdir"/usr/share/doc/"$i"
	done

	install -m644 logo_bot.png "$pkgdir"/$_basedir/logo_bot.png
	install -m644 logo_top.png "$pkgdir"/$_basedir/logo_top.png
	install -m644 monitorixico.png "$pkgdir"/$_basedir/monitorixico.png
	install -m644 docs/$_pkgname-alert.sh "$pkgdir"/usr/share/doc/$_pkgname-alert.sh
	install -m644 docs/$_pkgname-apache.conf "$pkgdir"/usr/share/doc/$_pkgname-apache.conf
	install -Dm644 docs/$_pkgname.logrotate "$pkgdir"/etc/logrotate.d/$_pkgname
	
	gzip -9 man/man8/$_pkgname.8 && gzip -9 man/man5/$_pkgname.conf.5
	install -Dm644 man/man5/$_pkgname.conf.5.gz "$pkgdir"/usr/share/man/man5/$_pkgname.conf.5.gz
	install -Dm644 man/man8/$_pkgname.8.gz "$pkgdir"/usr/share/man/man8/$_pkgname.8.gz
	install -dm777 "$pkgdir"/$_basedir/imgs

	for i in reports/*.html; do
		install -Dm644 "$i" "$pkgdir/$_libdir/reports/${i##reports/}"
	done
}
