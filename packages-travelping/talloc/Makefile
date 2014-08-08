#
# Copyright (C) 2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=talloc
PKG_VERSION:=2.1.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=http://www.samba.org/ftp/talloc/
PKG_MD5SUM:=85a69bfb5808fcadf378bd9eadb1f9c2

PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libtalloc
  SECTION:=libs
  CATEGORY:=Libraries
  TITLE:=libtalloc
  URL:=http://talloc.samba.org/
endef

define Package/libtalloc/description
 talloc is a hierarchical, reference counted memory pool system with
 destructors. It is the core memory allocator used in Samba.
endef

CONFIGURE_ARGS:= \
	--without-gettext \
	--disable-python

define Build/Configure
	$(CP) ./files/answers-32bit.txt $(PKG_BUILD_DIR)
	$(call Build/Configure/Default, --cross-compile --cross-answers=answers-32bit.txt)
endef

TARGET_CFLAGS += $(FPIC)
MAKE_FLAGS += CFLAGS_O="$(TARGET_CFLAGS)"

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/local/include/talloc.h $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/local/lib/libtalloc.so* $(1)/usr/lib/
endef

define Package/libtalloc/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/local/lib/libtalloc.so* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,libtalloc))
