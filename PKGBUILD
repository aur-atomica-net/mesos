# Maintainer: Jason R. McNeil <jason@jasonrm.net>
# Contributor: Daichi Shinozaki <dsdseg@gmail.com>

pkgname=mesos
pkgver=0.28.0
pkgrel=1
pkgdesc="Apache Mesos abstracts CPU, memory, storage, and other compute resources away from machines (physical or virtual), enabling fault-tolerant and elastic distributed systems to easily be built and run effectively."
arch=('i686' 'x86_64')
url="http://mesos.apache.org/"
license=('Apache')
install="mesos.install"
depends=('libnl>=3.2.26')
# depends=('python2' 'curl' 'leveldb' 'java-environment' 'libunwind' 'google-glog'
#          'libnl>=3.2.26' 'apr' 'subversion' 'protobuf' 'python2-protobuf' 'python2-boto')
# makedepends=('java-environment' 'maven' 'http-parser' 'python2-http-parser' 'google-glog'
#              'gperftools' 'apr' 'subversion' 'protobuf' 'python2-protobuf' 'python2-boto')
source=("http://www.apache.org/dist/mesos/${pkgver}/mesos-${pkgver}.tar.gz"
        "mesos-master.service"
        "mesos-slave.service"
        "mesos.install")

sha256sums=('7e4183ada965ac8a1345ef71cd284c9c2834ac57fe4c0794fdf1ea5bb291b615'
            'e6bee061ad26dcae87ea584dc970c1c682d3104648de08c8933a6cd19b3a0413'
            'ddfea9851177e22f06fa98fc5704e4c14ef284f2c5b4c9e53d01ff6a49febd5d'
            'fcb525b58905ce8f91bde7c8bcd2f9246bbab3e06b4fc208752d41f6d68a34f6')

prepare() {
  mkdir -p ${srcdir}/mesos-${pkgver}/build/.m2

  # fix python path (/usr/bin/env python -> python2)
  #find src/cli -type f -print | xargs sed --in-place -e "1 s/\(\/usr\/bin\/env python$\)/\12/"
}

build() {
  cd ${srcdir}/mesos-${pkgver}/build

  ../configure \
    --prefix=/usr \
    --sbindir=/usr/bin \
    --sysconfdir=/etc \
    --with-network-isolator

  make
}

# check() {
#   cd ${srcdir}/mesos-${pkgver}/build
#   # make -k check
# }

package() {
  cd ${srcdir}/mesos-${pkgver}/build
	make DESTDIR="${pkgdir}/" install

  mkdir -p -m755 ${pkgdir}/var/log/${pkgname}
  mkdir -p -m755 ${pkgdir}/var/lib/${pkgname}/{master,slave}

  mkdir -p -m755 ${pkgdir}/usr/lib/systemd/system
  install -m644 ${srcdir}/${pkgname}-{master,slave}.service ${pkgdir}/usr/lib/systemd/system

  ## Java
  # mkdir -p -m755 ${pkgdir}/usr/share/java/${pkgname}
  # install -m644 ./src/java/mesos.pom ${pkgdir}/usr/share/java/${pkgname}/
  # install -m644 ./src/java/target/*.jar ${pkgdir}/usr/share/java/${pkgname}/

  ## Python
  # cd ./src/python
  # python2 setup.py install --root="${pkgdir}" --optimize=1
}
