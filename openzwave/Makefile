#
# Copyright (C) 2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=openzwave
PKG_REV:=743
PKG_VERSION:=1.5.$(PKG_REV)
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/OpenZWave/open-zwave.git
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)
PKG_SOURCE_VERSION:=8c1073f017a6dc82dc6a07f0d46ffb4c597508f5

include $(INCLUDE_DIR)/package.mk

define Package/openzwave
  SECTION:=kai
  CATEGORY:=Domion
  TITLE:=Open ZWave Library.
  URL:=http://openzwave.com
  DEPENDS:=+udev
  MAINTAINER:=Ka <k@ka.st>
endef

define Package/openzwave/description
	open-zwave is a library interface for selected Z-Wave PC controllers
        Original Repo: AudiculaPi by irimi <irimi@gmx.de>
endef

define Package/openzwave/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/libopenzwave.so* $(1)/usr/lib/
	$(CP) $(PKG_BUILD_DIR)/libopenzwave.a $(STAGING_DIR)/usr/lib/
endef

$(eval $(call BuildPackage,openzwave))
