#
# Copyright (C) 2007-2017 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=seafile-ccnet
PKG_VERSION:=6.2.2
PKG_RELEASE=$(PKG_SOURCE_VERSION)-1
PKG_LICENSE:=GPL-3.0

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/haiwen/ccnet-server.git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=6b9d7e2920aa9b807f9efe9038439b57ce949ecc
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_MIRROR_HASH:=8aed6d2283ac7b3148f9b5c82b1d992ed36dc5193000cf727bdcfae7446ce1fe
PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/nls.mk
include ../../lang/python/python-package.mk

define Package/seafile-ccnet
    SECTION:=net
    CATEGORY:=Network
    TITLE:=Seafile server - ccnet component
    MAINTAINER:=Gergely Kiss <mail.gery@gmail.com>
    URL:=http://seafile.com/
    DEPENDS:=+libsearpc +libevent2 +libopenssl \
		+glib2 +python +libzdb +libuuid \
		+libpthread +libsqlite3 +jansson $(ICONV_DEPENDS)
    EXTRA_DEPENDS:=libsearpc (=6.2.2-8998e7b2c5587f0b94c48db24e2952d08def5add-1)
endef

define Package/seafile-ccnet/description
   Ccnet is a framework for writing networked applications in C.
endef

CONFIGURE_ARGS += --disable-client \
		    --enable-server \
		    --disable-ldap \
		    --disable-cluster \
		    --enable-python \
		    --disable-server-pkg \
		    --disable-static-build \
		    --disable-compile-demo \
		    --disable-console

PKG_BUILD_DEPENDS:=vala/host

TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib -liconv \
		    -L$(STAGING_DIR)/usr/lib/mysql -lmysqlclient -lz

define Package/seafile-ccnet/install
	$(INSTALL_DIR) $(1)/usr/{bin,lib}
	$(INSTALL_DIR) $(1)$(PYTHON_PKG_DIR)
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/ccnet* $(1)/usr/bin/
	$(CP) $(PKG_INSTALL_DIR)$(PYTHON_PKG_DIR)/ccnet $(1)$(PYTHON_PKG_DIR)
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libccnet.so* $(1)/usr/lib/
	find $(1) -name "*\.pyc" -o -name "*\.pyo" | xargs rm -f
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/lib/pkgconfig
	$(CP) $(PKG_INSTALL_DIR)/usr/include/ccnet* $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/pkgconfig/libccnet.pc $(1)/usr/lib/pkgconfig/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libccnet.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,seafile-ccnet))
