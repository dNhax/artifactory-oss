# Maintainer: Tobias Hübner <dasNeutrum at gmx dot de>

_pkgname=artifactory
pkgname=${_pkgname}-oss
pkgver=6.11.3
pkgrel=1
pkgdesc='An advanced Binary Repository Manager for use by build tools, dependency management tools and build servers'
arch=('x86_64')
url="https://bintray.com/jfrog/product/JFrog-Artifactory-Oss/view"
license=('AGPLv2')

depends=('java-runtime-headless' 'net-tools' 'bash')
optdepends=('apache: a fully featured webserver'
            'mysql: Fast SQL database server, community edition'
            'postgresql: A sophisticated object-relational DBMS')

backup=("etc/webapps/${_pkgname}/artifactory.config.xml"
        "etc/webapps/${_pkgname}/artifactory.system.properties"
        "etc/webapps/${_pkgname}/binarystore.xml"
        "etc/webapps/${_pkgname}/db.properties"
        "etc/webapps/${_pkgname}/logback.xml"
        "etc/webapps/${_pkgname}/mimetypes.xml")

install="$pkgname.install"
source=("jfrog-artifactory-oss-${pkgver}.zip::https://bintray.com/jfrog/artifactory/download_file?file_path=jfrog-artifactory-oss-${pkgver}.zip"
        'artifactory.service'
        'artifactory-user.conf'
        'artifactory-tmpfile.conf'
        'artifactory.default')
sha256sums=('fc2277fa4da9cfd83ca3af9ca94b2b03717e3df60573ab19f0281c9954117eda'
            'c7cc41af2479678e6fa605b91c20e6916f6cf374525e9d1552299bae5c5a2aaa'
            '2e6285bb5ab580a8f4a47580ffacfec9a537190d94c9fe11a2f82c6e65a9ba8a'
            'ae3ddc469e5c8702f97df262e65ca1f73f3fda22ee293cd6a7ba87a0e9162467'
            'cd0bef2a92751060b44b5e50815749b49152a66e7726d5f0c26889c0bc77a3d5')
options=('!strip')
PKGEXT='.pkg.tar'

package() {
    cd "${srcdir}/${pkgname}-${pkgver}"

    # Copy everything except etc and logs to /user/share/webapps/artifactory.
    install -dm755 "${pkgdir}/usr/share/webapps/${_pkgname}"
    cp -dr --no-preserve=ownership {bin,misc,tomcat,webapps} "${pkgdir}/usr/share/webapps/${_pkgname}/"

    # Install the license.
    install -Dm644 "COPYING.AFFERO" "${pkgdir}/usr/share/doc/${_pkgname}/COPYING"

    # Delete unused files.
    rm -f ${pkgdir}/usr/share/webapps/${_pkgname}/bin/*.{exe,bat}
    rm -f ${pkgdir}/usr/share/webapps/${_pkgname}/bin/{install,uninstall}Service.sh
    rm -f ${pkgdir}/usr/share/webapps/${_pkgname}/bin/artifactoryctl
    rm -f ${pkgdir}/usr/share/webapps/${_pkgname}/bin/metadata/metadata-api-windows-amd64.exe
    rm -f ${pkgdir}/usr/share/webapps/${_pkgname}/bin/metadata/metadata-api-darwin-amd64
    rm -f ${pkgdir}/usr/share/webapps/${_pkgname}/tomcat/bin/*.bat
    rm -f ${pkgdir}/usr/share/webapps/${_pkgname}/COPYING* *.txt *.html

    # Install the configuration files to /etc/webapps/artifactory.
    install -Dm644 "etc/artifactory.config.xml" "${pkgdir}/etc/webapps/${_pkgname}/artifactory.config.xml"
    install -Dm644 "etc/artifactory.system.properties" "${pkgdir}/etc/webapps/${_pkgname}/artifactory.system.properties"
    install -Dm644 "etc/binarystore.xml" "${pkgdir}/etc/webapps/${_pkgname}/binarystore.xml"
    install -Dm644 "etc/logback.xml" "${pkgdir}/etc/webapps/${_pkgname}/logback.xml"
    install -Dm644 "etc/mimetypes.xml" "${pkgdir}/etc/webapps/${_pkgname}/mimetypes.xml"

    # Install the default configuration
    cd "${srcdir}"
    install -Dm644 "${_pkgname}.default" "${pkgdir}/usr/share/webapps/${_pkgname}/bin"

    # Install the systemd configuration and service files
    install -Dm644 "${_pkgname}.service" "${pkgdir}/usr/lib/systemd/system/${_pkgname}.service"
    install -Dm644 "${_pkgname}-user.conf" "${pkgdir}/usr/lib/sysusers.d/${_pkgname}.conf"
    install -Dm644 "${_pkgname}-tmpfile.conf" "${pkgdir}/usr/lib/tmpfiles.d/${_pkgname}.conf"

    # Create symbolic links because Artifactory expects a specific directory layout.
    ln -s "/var/log/${_pkgname}" "${pkgdir}/usr/share/webapps/${_pkgname}/logs"
    ln -s "/run/${_pkgname}" "${pkgdir}/usr/share/webapps/${_pkgname}/run"
    ln -s "/etc/webapps/${_pkgname}" "${pkgdir}/usr/share/webapps/${_pkgname}/etc"
}
