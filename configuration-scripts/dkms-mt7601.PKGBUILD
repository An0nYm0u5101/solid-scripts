# Maintainer: Yong-hyu Ban <maneulyori@gmail.com>
pkgname=dkms-mt7601u
pkgver=v3.0.0.4
pkgrel=1
pkgdesc="Driver for Ralink MT7601U chipset wireless adaptors"
arch=('i686' 'x86_64')
url="http://www.ralinktech.com"
license=('GPL')
depends=('dkms' 'linux-headers')
conflicts=()
install=${pkgname}.install
options=(!strip)
source=("http://www.mediatek.com/AmazonS3/Downloads/linux/DPO_MT7601U_LinuxSTA_3.0.0.4_20130913.tar.bz2"
        "kuid_t.patch::http://up.levert.ch/d7ba21-patch314.patch"
        "dkms.conf")

md5sums=('5f440dccc8bc952745a191994fc34699'
         '6aecede18cb218e108464d9d73fff3d5'
         '95caf4e6ba262c9683867e207fe846d7')


prepare() {
    rm -rf ${srcdir}/$pkgname-$pkgver
    # Change src dir name
    mv ${srcdir}/DPO_MT7601U_LinuxSTA_3.0.0.4_20130913 ${srcdir}/$pkgname-$pkgver

    cd "${srcdir}/${pkgname}-${pkgver}/"
    patch -Np1 -i ${srcdir}/kuid_t.patch
}

build() {
  DATE=$(date +%d-%m-%Y);
  TIME=$(date +%z);

  #gcc 4.9 adds -Werror=date-time, which Linux has enabled in its build.
  cd "${srcdir}/${pkgname}-${pkgver}/"
  sed -i "s|__DATE__|\"$DATE\"|g" sta/sta_cfg.c
  sed -i "s|__TIME__|\"$TIME\"|g" sta/sta_cfg.c
}

package() {

    installDir="$pkgdir/usr/src/$pkgname-$pkgver"

    install -dm755 "$installDir"
    install -m644 "$srcdir/dkms.conf" "$installDir"
    install -dm755 "$pkgdir/etc/modprobe.d"

    cd "${srcdir}/${pkgname}-${pkgver}/"

    for d in `find . -type d`
    do
install -dm755 "$installDir/$d"
    done

for f in `find . -type f -o -type l`
    do
install -m644 "${srcdir}/${pkgname}-${pkgver}/$f" "$installDir/$f"
    done
}
