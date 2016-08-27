#
# Copyright (C) 2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_REV:=1
PKG_NAME:=agocontrol
PKG_VERSION:=1.${PKG_REV}
PKG_RELEASE:=1
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=http://git.agocontrol.com/agocontrol/agocontrol.git
PKG_SOURCE_VERSION:=3ca62c896f5699be99d6f85ab9068254f15f771e 
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)

CMAKE_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/agocontrol
  SECTION:=ka
  CATEGORY:=Domion
  TITLE:=Agocontrol Home Automation 
  DEPENDS:=+libubacktrace +zlib +libusb-1.0 +libusb-compat +libcurl +libsqlite3 +libopenssl +boost +boost-system +boost-thread +boost-date_time +AGOCONTROL_OZW_SUPPORT:openzwave
  URL:=https://agocontrol.com/
  MAINTAINER:=Ka <k@ka.st>
endef

define Package/agocontrol/description
You want to rock your home with automation - ago control can do it! And it is free (GPL licensed)!
endef


define Package/agocontrol/config
  menu "Configuration"
  depends on PACKAGE_domoticz
  source "$(SOURCE)/Config.in"
  endmenu
endef
        
CMAKE_OPTIONS += \
	-DCMAKE_BUILD_TYPE=Release . \
	-DCPACK_STRIP_FILES=1
#	-DDEBUG=1

define Package/agocontrol/install
#	$(INSTALL_DIR) $(1)/etc/init.d
#	$(INSTALL_DIR) $(1)/opt/agocontrol
#	$(CP) $(PKG_INSTALL_DIR)/usr/domoticz $(1)/opt/domoticz/
#	$(CP) $(PKG_INSTALL_DIR)/usr/www $(1)/opt/domoticz/
#	$(CP) $(PKG_INSTALL_DIR)/usr/Config $(1)/opt/domoticz/
#	$(CP) $(PKG_INSTALL_DIR)/usr/scripts $(1)/opt/domoticz/
#	$(INSTALL_BIN) ./files/domoticz.init $(1)/etc/init.d/domoticz
endef

######################################
#
# Important: if backtrace functionality is enabled in domoticz uClibc openwrt's settings for uCkibc must be changed:
# LIBC_HAS_BACKTRACE=y
# resp see file [openwrt_root]/toolchain/uClibc/config-0.9.33.2/common
# rebuild uClibc with make ./toolchain/uClibc/{clean, compile} V=s
#
#######################################
define Package/libubacktrace
$(call Package/libc/Default)
	TITLE:=Backtrace extension library
	DEPENDS:=+libpthread
endef

define Package/libubacktrace/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(TOOLCHAIN_DIR)/lib/libubacktrace.so.* $(1)/usr/lib/
	$(CP) $(TOOLCHAIN_DIR)/lib/libubacktrace-* $(1)/usr/lib/
endef
                    
                        
$(eval $(call BuildPackage,agocontrol))
$(eval $(call BuildPackage,libubacktrace))
