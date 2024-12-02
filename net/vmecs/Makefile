#
# Copyright (C) 2006-2014 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=vmecs
PKG_VERSION:=1.5.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/Lankaster/vmecs.git
PKG_HASH:=0fda4b6f8ce43df41546c877a3099be771095740a07116b36aa2902cc20e938e

PKG_MAINTAINER:=Yaroslav Petrov <info@lank.me>
PKG_LICENSE:=GPL-3.0
PKG_LICENSE_FILES:=LICENSE
PKG_CPE_ID:=cpe:/a:V2Ray:vmecs

PKG_INSTALL:=1
PKG_BUILD_PARALLEL:=1

include $(TOPDIR)/rules.mk

PKG_CONFIG_DEPENDS:= \
	CONFIG_VMECS_GNUTLS \
	CONFIG_VMECS_OPENSSL \
	CONFIG_VMECS_MBEDTLS

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/vmecs
	SECTION:=net
	CATEGORY:=Network
	SUBMENU:=VPN
	TITLE:=Vmecs
	DEPENDS:= +VMECS_OPENSSL:libopenssl +VMECS_GNUTLS:libgnutls +VMECS_MBEDTLS:libmbedtls +libpthread
	MENU:=1
endef

define Package/vmecs/description
	vmecs is a proxy implementation written in C, and is based on VMess protocol which is designed
	by V2Ray project (a.k.a Project V)
endef

define Package/vmecs/config
	source "$(SOURCE)/Config.in"
endef

CMAKE_OPTIONS += \
	$(if $(CONFIG_VMECS_OPENSSL),-DUSE_OPENSSL=on) \
	$(if $(CONFIG_VMECS_MBEDTLS),-DUSE_MBEDTLS=on) \
	$(if $(CONFIG_VMECS_GNUTLS),-DUSE_GNUTLS=on)

define Package/vmecs/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/lib*.so $(1)/usr/lib/
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/socks5-client $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/socks5-server $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/toml-test $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vhook $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vmecs $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vmess-client $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vmess-middle $(1)/usr/bin/
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vmess-server $(1)/usr/bin/
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/vmecs.config $(1)/etc/config/vmecs
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/vmecs.init $(1)/etc/init.d/vmecs
endef


$(eval $(call BuildPackage,$(PKG_NAME)))
