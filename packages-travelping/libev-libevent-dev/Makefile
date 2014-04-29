#
# Copyright (C) 2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=libev-libevent-dev
PKG_VERSION:=4.11_1
PKG_RELEASE:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libev-libevent-dev
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=libevent event loop compatibility wrapper for libev
  DEPENDS:=+libev
  URL:=http://software.schmorp.de/pkg/libev.html
endef

define Package/libevent/description
 Compatibility wrapper "event.h" to enable programs which use the libevent
 event loop to link against libev.
endef

define Build/Prepare
	$(INSTALL_DIR) $(1)/usr/include/
	$(CP) \
		./files/event.h \
		$(1)/usr/include/
endef

define Build/Configure
endef

define Build/Compile
endef

define Build/Install
endef

define Package/libev-libevent-dev/install
	true
endef

$(eval $(call BuildPackage,libev-libevent-dev))
