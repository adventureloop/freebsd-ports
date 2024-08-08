PORTNAME=	vpp
DISTVERSION=	24.06
DISTVERSIONPREFIX=	v
CATEGORIES=	net
#MASTER_SITES=	

PATCH_SITES=	https://github.com/adventureloop/${PORTNAME}/commit/
PATCHFILES=	fba2754d78cde15f93d19f86499aa3803110bc73.patch:-p1
PATCHFILES+=	c15618aa59bf39197c66ec2e82c58c02d4c56c20.patch:-p1

MAINTAINER=	thj@freebsd.org
COMMENT=	VPP: A fast, scalable layer 2-4 multi-platform network stack.
WWW=		https://fd.io

LICENSE=	APACHE20
LICENSE_FILE=	${WRKSRC}/LICENSE

USE_GITHUB=	yes
GH_ACCOUNT=	FDio
GH_PROJECT=	vpp
# This is the tested commit on the stable/2406 branch
GH_TAGNAME=	cd30ea1ddfc320965cbf01d02bed2f370bb11504

#USES= 		gmake ninja:build pkgconfig python:build
USES= 		gmake ninja:build python:build shebangfix ssl
SHEBANG_REGEX=  '.*\.py$$'
ALL_TARGET=	build-release

#BUILD_DEPENDS=	jansson:devel/jansson
#LIB_DEPENDS=	libepoll-shim.so:devel/libepoll-shim
USE_LDCONFIG=	yes
BUILD_DEPENDS=	bash:shells/bash \
		cmake:devel/cmake-core \
		${PYTHON_PKGNAMEPREFIX}ply>0:devel/py-ply@${PY_FLAVOR}
LIB_DEPENDS=	libepoll-shim.so:devel/libepoll-shim \
		libpcap.so:net/libpcap \
		librte_cryptodev.so.23:net/dpdk

VPPPROGS=	vat2 vpp_json_test vpp_echo vpp_prometheus_export svmdbtool \
		vpp_get_metrics vpp_restart svmtool vpp_get_stats \
		vpp vppctl 
VPPBIN=		/build-root/build-vpp-native/vpp/bin
VPPLIB=		/build-root/build-vpp-native/vpp/lib

do-install:
	# vpp_find_plugin_path hard codes in a search based on the vpp path to
	# find vpp_plugsins and explicitly looks for 'bin' (src/vpp/vnet/main.c)
.for vpp_prog in ${VPPPROGS}
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/${vpp_prog} ${STAGEDIR}${PREFIX}/bin
.endfor
	${MKDIR} ${STAGEDIR}${PREFIX}/lib/vpp_plugins ${STAGEDIR}${PREFIX}/lib/vat2_plugins
	${CP} ${WRKSRC}${VPPLIB}/vpp_plugins/* ${STAGEDIR}${PREFIX}/lib/vpp_plugins/
	${CP} ${WRKSRC}${VPPLIB}/vat2_plugins/* ${STAGEDIR}${PREFIX}/lib/vat2_plugins/
	${INSTALL_LIB} ${WRKSRC}${VPPLIB}/*.so* ${STAGEDIR}${PREFIX}/lib

post-install:
	${FIND} ${STAGEDIR}${PREFIX}/lib/ -name *.so -exec ${STRIP_CMD} {} +

.include <bsd.port.mk>

