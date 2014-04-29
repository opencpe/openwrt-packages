#
# Copyright (C) 2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=mand-cfgd
PKG_VERSION:=2014-04-29
PKG_REV:=e46d4f1e31af172b804f538bbadcb2a05ec1c163
PKG_RELEASE:=2

PKG_SOURCE:=mand-cfgd-$(PKG_REV).tar.gz
PKG_SOURCE_URL:=git@github.com:travelping/mand-cfgd.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/mand-cfgd
  SECTION:=travelping
  CATEGORY:=Travelping
  TITLE:=mand-cfgd
  DEPENDS:=+mand +ip +libnl
endef

define Package/mand-cfgd/description
 Config agent
endef

TARGET_CFLAGS += \
	-std=gnu99

CONFIGURE_VARS += \
	CFLAGS="$(TARGET_CFLAGS)"

define Package/mand-cfgd/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(CP) $(PKG_INSTALL_DIR)/usr/bin/mand-cfgd $(1)/usr/bin

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) \
		./files/mand-cfgd.init \
		$(1)/etc/init.d/mand-cfgd
endef

$(eval $(call BuildPackage,mand-cfgd))
