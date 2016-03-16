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

CONFIGURE_VARS += \
        ac_cv_func_newlocale=no

define Package/netvirt
  SECTION:=ka
  CATEGORY:=Domion
  TITLE:=Netvirt VPN 
  DEPENDS:=+libopenssl +libpthread +libstdcpp
  URL:=https://netvirt.org/
  MAINTAINER:=Ka <k@ka.st>
endef

define Package/netvirt/description
  NetVirt is a network virtualization platform that allows you to create and manage your
  own peer-to-peer virtual private network (VPN) with the goal to keep it simple for everyone. VPN and
  encryption are often only used by those who are knowledgeable and more skilled. Everyone
  should be able to use a VPN-based technology. 
endef

define Build/Prepare
	$(PKG_UNPACK)
	$(INSTALL_BIN) ./files/xlocale.h $(PKG_BUILD_DIR)/xlocale.h
	$(Build/Patch)
	sed "s/mips-openwrt-linux-uclibc-/$(TARGET_CROSS)/g" -i $(PKG_BUILD_DIR)/tapcfg/SConstruct
endef

define Build/Compile
	cd $(PKG_BUILD_DIR)/udt4 && $(MAKE) CXX="$(TARGET_CROSS)g++"  CC="$(TARGET_CROSS)gcc"  arch=mips
   
	cd $(PKG_BUILD_DIR)/libconfig && \
		./configure --disable-examples \
		--disable-cxx \
		ac_cv_func_newlocale=no \
		ac_cv_func_freelocale=no \
		LD="$(TARGET_CROSS)ld"  && \
		$(MAKE) CXX="$(TARGET_CROSS)g++" \
		CC="$(TARGET_CROSS)gcc" \
		LD="$(TARGET_CROSS)ld"  \
		arch=mips \
		CFLAGS+="-I$(PKG_BUILD_DIR)/"

	cd $(PKG_BUILD_DIR)/tapcfg && mkdir -p release && \
		scons -C $(PKG_BUILD_DIR)/tapcfg \
		$(PKG_JOBS) \
		WS=off \
		CPU=openwrt \
		OS=openwrt \
		BR=off \
		ICE=off \
		POLICYDB=on && \
		mv $(PKG_BUILD_DIR)/tapcfg/build/libtapcfg.so $(PKG_BUILD_DIR)/tapcfg/release/libtapcfg32.so && \
		$(STRIP) $(PKG_BUILD_DIR)/tapcfg/release/libtapcfg32.so

	mkdir -p $(PKG_BUILD_DIR)/build.linux.cli && \
		cd $(PKG_BUILD_DIR)/build.linux.cli && \
		cmake .. -DWITH_GUI=OFF && \
		cd .. && make netvirt-agent
endef

define Package/netvirt/install
	$(INSTALL_DIR) $(1)/usr/bin
#	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_BUILD_DIR)/nvagent/src/netvirt-agent $(1)/usr/bin/netvirt-agent 
#	$(CP) $(PKG_BUILD_DIR)/udt4/src/libudt.a $(1)/usr/lib/libudt.a
#	$(CP) $(PKG_BUILD_DIR)/udt4/src/libudt.so $(1)/usr/lib/libudt.so
#	$(CP) $(PKG_BUILD_DIR)/tapcfg/build/libtapcfg.a $(1)/usr/lib/libtapcfg.a
#	$(CP) $(PKG_BUILD_DIR)/tapcfg/release/libtapcfg32.so $(1)/usr/lib/libtapcfg32.so
endef

CMAKE_OPTIONS += \
	-DCMAKE_BUILD_TYPE=Release . \
	-DCPACK_STRIP_FILES=1 \
	-DWITH_GUI=OFF

                        
$(eval $(call BuildPackage,netvirt))
