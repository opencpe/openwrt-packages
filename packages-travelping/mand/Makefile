#
# Copyright (C) 2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/version.mk

PKG_NAME:=mand
PKG_VERSION:=2014-04-30
PKG_REV:=27ec8f84a7b336dee10a03d118db0d5ab9569b3b
PKG_RELEASE:=1

PKG_SOURCE:=mand-$(PKG_REV).tar.gz
PKG_SOURCE_URL:=git://github.com/opencpe/mand
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_BUILD_DEPENDS:=luac/host

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/mand
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=mand
  DEPENDS:=+liblua +libev +libtalloc +libexpat +librt +libpthread +libopenssl +zlib
endef

TARGET_CFLAGS += \
	-std=gnu99

CONFIGURE_VARS += \
	CFLAGS="$(TARGET_CFLAGS)"

CONFIGURE_ARGS += \
	--enable-lua-functions=/usr/share/mand \
	--enable-lua-libdir=/usr/lib/lua

define Package/mand/description
 Device Management Daemon
endef

define Package/mand/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdmconfig.so.* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdmutils.so.* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdm_dmclient.so.* $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdm_dmconfig.so.* $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/sbin
	$(CP) $(PKG_INSTALL_DIR)/usr/sbin/{dmctrl,mand} $(1)/usr/sbin

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) \
		./files/mand.init \
		$(1)/etc/init.d/mand

	$(INSTALL_DIR) $(1)/etc/dm
	$(CP) ./files/dm.xml $(1)/etc/dm
	$(VERSION_SED) $(1)/etc/dm/dm.xml

	$(INSTALL_DIR) $(1)/jffs/etc
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/{libdmconfig,mand} $(1)/usr/include
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdmconfig.{a,so*} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdmutils.{a,so*} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdm_dmclient.{a,so*} $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdm_dmconfig.{a,so*} $(1)/usr/lib/
endef

$(eval $(call BuildPackage,mand))
