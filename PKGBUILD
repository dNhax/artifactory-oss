# Maintainer: Tobias Hübner <dasNeutrum@gmx.de>

pkgname=artifactory-oss
pkgver=7.12.8
pkgrel=1
pkgdesc='An advanced Binary Repository Manager for use by build tools, dependency management tools and build servers'
arch=('x86_64')
url='https://bintray.com/jfrog/product/JFrog-Artifactory-Oss/view'
license=('AGPLv2')

depends=(
    'java-runtime=11'
    'inetutils'
    'net-tools'
)
optdepends=(
    'nginx: Lightweight HTTP server and IMAP/POP3 proxy server (stable)'
    'nginx-mainline: Lightweight HTTP server and IMAP/POP3 proxy server (mainline)'
    'postgresql: Sophisticated object-relational DBMS'
)

backup=(
    "etc/webapps/${pkgname}/system.yaml"
# TODO
#     "etc/webapps/${pkgname}/artifactory/..."
)

install="${pkgname}.install"
source=(
    "jfrog-${pkgname}-${pkgver}-linux.tar.gz::https://jfrog.bintray.com/artifactory/org/artifactory/oss/jfrog-${pkgname}/${pkgver}/jfrog-${pkgname}-${pkgver}-linux.tar.gz"
    "${pkgname}.service"
    "${pkgname}-user.conf"
    "${pkgname}-tmpfile.conf"
)
sha256sums=('e86be6d1beb3c291506cb119233703826a7678479322bd70a54e61ec3a90aadc'
            '373c9b110263b98d0d60e3e432110bc2ae15e819fdb722fa2888c3c1dfd47891'
            '2e6285bb5ab580a8f4a47580ffacfec9a537190d94c9fe11a2f82c6e65a9ba8a'
            'dbc02c8e022c05bdc6ba6bf3e56b7be800142a0e0ad068db27a5b0d3f0a9dc9d')

options=('!strip')
PKGEXT='.pkg.tar'

package() {
    cd "${srcdir}/${pkgname}-${pkgver}"

    # Install everything to /usr/share/webapps/artifactory-oss
    install -dm755 "${pkgdir}/usr/share/webapps/${pkgname}"
    cp -dr --no-preserve=ownership "app" "${pkgdir}/usr/share/webapps/${pkgname}/"
    sed -i "s/export JF_ARTIFACTORY_PID=.*/export JF_ARTIFACTORY_PID=\/var\/run\/artifactory-oss\/artifactory\.pid/g" "${pkgdir}/usr/share/webapps/artifactory-oss/app/bin/artifactoryManage.sh"

    install -dm755 "${pkgdir}/usr/share/webapps/${pkgname}/var"
    cp -dr --no-preserve=ownership "var/bootstrap" "${pkgdir}/usr/share/webapps/${pkgname}/var/"

    # Install the license
    install -Dm644 "app/doc/COPYING.AFFERO" "${pkgdir}/usr/share/doc/${pkgname}/COPYING"

    # Install the configuration files to /etc/webapps/artifactory-oss
    install -dm750 "${pkgdir}/etc/webapps/${pkgname}"
    cp -dr --no-preserve=ownership "var/etc/." "${pkgdir}/etc/webapps/${pkgname}/"

    # Install the default configuration
    install -Dm644 "var/etc/system.full-template.yaml" "${pkgdir}/etc/webapps/${pkgname}/system.yaml"

    # Install the systemd configuration and service files
    cd "${srcdir}"
    install -Dm644 "${pkgname}.service" "${pkgdir}/usr/lib/systemd/system/${pkgname}.service"
    install -Dm644 "${pkgname}-user.conf" "${pkgdir}/usr/lib/sysusers.d/${pkgname}.conf"
    install -Dm644 "${pkgname}-tmpfile.conf" "${pkgdir}/usr/lib/tmpfiles.d/${pkgname}.conf"

    # Create symbolic links because Artifactory expects a specific directory layout
    ln -s "/etc/webapps/${pkgname}" "${pkgdir}/usr/share/webapps/${pkgname}/var/etc"
    ln -s "/var/log/${pkgname}" "${pkgdir}/usr/share/webapps/${pkgname}/var/log"
    ln -s "/run/${pkgname}" "${pkgdir}/usr/share/webapps/${pkgname}/app/run"
}
