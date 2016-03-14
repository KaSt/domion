#
# Copyright (C) 2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_REV:=1673
PKG_NAME:=netvirt
PKG_VERSION:=1.${PKG_REV}
PKG_RELEASE:=1
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2

PKG_SOURCE_PROTO:=git
PKG_SOURCE_URL:=https://github.com/netvirt/netvirt.git
PKG_SOURCE_VERSION:=cb5ad4e309728a22bc41464b28d79f1d33163ac0
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)

CMAKE_INSTALL:=1

include $(INCLUDE_DIR)/package.mk
include $(INCLUDE_DIR)/cmake.mk

define Package/netvirt
  SECTION:=ka
  CATEGORY:=Domion
  TITLE:=Netvirt VPN 
  DEPENDS:=+libopenssl
  URL:=https://netvirt.org/
  MAINTAINER:=Ka <k@ka.st>
endef

define Package/netvirt/description
  NetVirt is a network virtualization platform that allows you to create and manage your
  own peer-to-peer virtual private network (VPN) with the goal to keep it simple for everyone. VPN and
  encryption are often only used by those who are knowledgeable and more skilled. Everyone
  should be able to use a VPN-based technology. 
endef


CMAKE_OPTIONS += \
	-DCMAKE_BUILD_TYPE=Release . \
	-DCPACK_STRIP_FILES=1 \
	-DWITH_GUI=OFF
#	-DDEBUG=1

                        
$(eval $(call BuildPackage,netvirt))
