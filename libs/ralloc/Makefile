#
# Copyright (C) 2011-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ralloc
PKG_VERSION:=10.3.0
PKG_RELEASE:=1

PKG_SOURCE:=MesaLib-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=ftp://ftp.freedesktop.org/pub/mesa/10.3/
PKG_BUILD_DIR:=$(BUILD_DIR)/Mesa-$(PKG_VERSION)
PKG_MD5SUM:=bc071575596a074df2b15cac57c01ed8

PKG_LICENSE:=MIT

include $(INCLUDE_DIR)/package.mk

define Package/libralloc
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=MIT-x licenced talloc replacement
endef

define Package/libralloc/description
endef

define Build/Configure
	/bin/true
endef

define Build/Compile
	$(TARGET_CC) $(TARGET_CFLAGS) -Wall -shared \
		-o $(PKG_BUILD_DIR)/libralloc.so $(PKG_BUILD_DIR)/src/util/ralloc.c
endef

define Package/libralloc/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libralloc.so $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_BUILD_DIR)/src/util/ralloc.h $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/libralloc.so $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libralloc))
