PORTNAME=	vpp
DISTVERSION=	2024.06
CATEGORIES=	net
MASTER_SITES=	

#PATCH_SITES=	https://github.com/adventureloop/${PORTNAME}/commit/
#PATCHFILES=	669b3815e2ca7aa0653d8ca567b25165b3a75738.patch:-p1

MAINTAINER=	thj@freebsd.org
COMMENT=	VPP: A fast, scalable layer 2-4 multi-platform network stack.
WWW=		https://fd.io

LICENSE=	APACHE20
LICENSE_FILE=	${WRKSRC}/LICENSE

USE_GITHUB=	yes
GH_ACCOUNT=	adventureloop
GH_PROJECT=	vpp
# this should be a tag once we run in a release
# https://github.com/adventureloop/vpp/archive/62a61769a5be442b8e7de31264f2e56b49fa5024.tar.gz
GH_TAGNAME=	62a61769a5be442b8e7de31264f2e56b49fa5024

USES= 		gmake python ssl
ALL_TARGET=	build-release

LIB_DEPENDS=	libjansson.so:devel/jansson
LIB_DEPENDS+= 	librte_net.so:net/dpdk
BUILD_DEPENDS+= autoconf:devel/autoconf
BUILD_DEPENDS+= automake:devel/autoconf
BUILD_DEPENDS+= cmake:devel/cmake
BUILD_DEPENDS+= gsed:textproc/gsed
BUILD_DEPENDS+= curl:ftp/curl
BUILD_DEPENDS+= git:devel/git
BUILD_DEPENDS+= gmake:devel/gmake
BUILD_DEPENDS+= pkgconf:devel/pkgconf
BUILD_DEPENDS+= ${PYTHON_PKGNAMEPREFIX}ply>0:devel/py-ply@${PY_FLAVOR}
BUILD_DEPENDS+= ninja:devel/ninja

#RUN_DEPENDS=
USE_LDCONFIG=	yes

LIB_DEPENDS+=	libepoll-shim.so:devel/libepoll-shim
LIB_DEPENDS+=	libpcap.so:net/libpcap

VPPPROGS=	vat2 vpp_json_test vpp_echo vpp_prometheus_export svmdbtool \
		vpp_get_metrics vpp_restart svmtool vpp_get_stats \
		vpp vppctl 
VPPBIN=		/build-root/build-vpp-native/vpp/bin
VPPLIB=		/build-root/build-vpp-native/vpp/lib

do-install:
	# vpp_find_plugin_path hard codes in a search based on the vpp path to
	# find vpp_plugsins and explicitly looks for 'bin' (src/vpp/vnet/main.c)
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vat2 ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vpp_json_test ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vpp_echo ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vpp_prometheus_export ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/svmdbtool ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vpp_get_metrics ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vpp_restart ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/svmtool ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vpp_get_stats ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vpp ${STAGEDIR}${PREFIX}/bin
	${INSTALL_PROGRAM} ${WRKSRC}${VPPBIN}/vppctl ${STAGEDIR}${PREFIX}/bin

	${INSTALL_LIB} ${WRKSRC}${VPPLIB}/*.so* ${STAGEDIR}${PREFIX}/lib

post-install:

	${MKDIR} ${STAGEDIR}/lib/vpp_plugins
	${MKDIR} ${STAGEDIR}/lib/vat2_plugins

	${CP} ${WRKSRC}${VPPLIB}/vpp_plugins/* ${STAGEDIR}/lib/vpp_plugins/
	${CP} ${WRKSRC}${VPPLIB}/vat2_plugins/* ${STAGEDIR}/lib/vat2_plugins/

.include <bsd.port.mk>
