# 
# Copyright (C) 2009-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

SERVAL_VERSION:=batphone-release-0.90
COMMOTION_VERSION:=commotion-0.1

PKG_NAME:=olsrd
PKG_VERSION:=0.6.5.4-$(COMMOTION_VERSION)
PKG_RELEASE:=1

PKG_SOURCE_URL:=https://github.com/opentechinstitute/olsrd/archive
PKG_SOURCE_VERSION:=$(COMMOTION_VERSION)

PKG_SOURCE:=$(PKG_SOURCE_VERSION).tar.gz
PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)-$(PKG_SOURCE_VERSION)

PKG_BUILD_PARALLEL:=1
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

TARGET_CFLAGS += $(FPIC) -idirafter$(BUILD_DIR)/serval-dna-$(SERVAL_VERSION) -I$(BUILD_DIR)/serval-dna-$(SERVAL_VERSION)/nacl/include
TARGET_LDFLAGS += -L$(BUILD_DIR)/serval-dna-$(SERVAL_VERSION)/

define Package/olsrd/template
  SECTION:=net
  CATEGORY:=Network
  SUBMENU:=Routing and Redirection
  MAINTAINER:=Saverio Proto <zioproto@gmail.com>
  TITLE:=OLSR (Optimized Link State Routing) daemon
  URL:=http://www.olsr.org/
endef

define Package/olsrd
  $(call Package/olsrd/template)
  MENU:=1
  DEPENDS:=+libpthread
endef

define Package/olsrd/conffiles
/etc/config/olsrd
endef

define Package/olsrd-mod-arprefresh
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Kernel ARP cache refresh plugin
endef

define Package/olsrd-mod-dot-draw
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Dot topology information plugin
endef

define Package/olsrd-mod-bmf
  $(call Package/olsrd/template)
  DEPENDS:=olsrd +kmod-tun
  TITLE:=Basic multicast forwarding plugin
endef

define Package/olsrd-mod-dnssd
  $(call Package/olsrd/template)
  DEPENDS:=olsrd +libldns
  TITLE:=Forward mDNS/DNS-SD packets with custom TTL values
  MAINTAINER:=Dan Staples <danstaples AT opentechinstitute DOT org>
endef

define Package/olsrd-mod-dyn-gw
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Dynamic internet gateway plugin
endef

define Package/olsrd-mod-dyn-gw-plain
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Dynamic internet gateway plain plugin
endef

define Package/olsrd-mod-httpinfo
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Small informative web server plugin
endef

define Package/olsrd-mod-jsoninfo
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=jsoninfo
endef

define Package/olsrd-mod-mdns
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Multicast DNS plugin
endef

define Package/olsrd-mod-mdp
  $(call Package/olsrd/template)
  DEPENDS:=olsrd +serval-dna
  TITLE:=Sign olsrd traffic with a Serval key
  MAINTAINER:=Will Hawkins <hawkinsw AT opentechinstitute DOT org>
endef

define Package/olsrd-mod-nameservice
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Lightweight hostname resolver plugin
endef

define Package/olsrd-mod-p2pd
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Peer to Peer Discovery plugin
endef

define Package/olsrd-mod-pgraph
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=pgraph
endef

define Package/olsrd-mod-quagga
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Quagga plugin
endef

define Package/olsrd-mod-secure
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Message signing plugin to secure routing domain
endef

define Package/olsrd-mod-sgwdynspeed
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=sgwdynspeed
endef

define Package/olsrd-mod-txtinfo
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Small informative web server plugin
endef

define Package/olsrd-mod-watchdog
  $(call Package/olsrd/template)
  DEPENDS:=olsrd
  TITLE:=Watchdog plugin
endef

define Package/olsrd-mod-secure/conffiles
/etc/olsrd.d/olsrd_secure_key
endef

MAKE_FLAGS+= \
	NO_DEBUG_MESSAGES=1 \
	OS="linux" \
	DESTDIR="$(PKG_INSTALL_DIR)" \
	STRIP="true" \
	INSTALL_LIB="true" \
	SUBDIRS="arprefresh bmf dnssd dot_draw dyn_gw dyn_gw_plain httpinfo jsoninfo mdns mdp nameservice p2pd pgraph quagga secure sgwdynspeed txtinfo watchdog"

define Build/Compile
	$(call Build/Compile/Default,all)
	$(call Build/Compile/Default,libs)
endef

define Build/Install
	$(call Build/Install/Default,install)
	$(call Build/Install/Default,install_libs)
endef

define Package/olsrd/install
	$(INSTALL_DIR) $(1)/etc/config
	$(INSTALL_DATA) ./files/olsrd.config $(1)/etc/config/olsrd
	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/olsrd $(1)/usr/sbin/
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/olsrd.init $(1)/etc/init.d/olsrd
endef

define Package/olsrd-mod-arprefresh/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/arprefresh/olsrd_arprefresh.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-dot-draw/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/dot_draw/olsrd_dot_draw.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-bmf/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/bmf/olsrd_bmf.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-dnssd/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/dnssd/olsrd_dnssd.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-dyn-gw/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/dyn_gw/olsrd_dyn_gw.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-dyn-gw-plain/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/dyn_gw_plain/olsrd_dyn_gw_plain.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-httpinfo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/httpinfo/olsrd_httpinfo.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-jsoninfo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/jsoninfo/olsrd_jsoninfo.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-mdns/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/mdns/olsrd_mdns.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-mdp/install
	$(INSTALL_DIR) $(1)/etc/commotion/keys.d/mdp
	$(CP) ./files/mdp.keyring $(1)/etc/commotion/keys.d/mdp/serval.keyring
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/mdp/olsrd_mdp.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-nameservice/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/nameservice/olsrd_nameservice.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-p2pd/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/p2pd/olsrd_p2pd.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-pgraph/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/pgraph/olsrd_pgraph.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-quagga/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/quagga/olsrd_quagga.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-secure/install
	$(INSTALL_DIR) $(1)/etc/olsrd.d
	$(CP) ./files/olsrd_secure_key $(1)/etc/olsrd.d/
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/secure/olsrd_secure.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-sgwdynspeed/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/sgwdynspeed/olsrd_sgwdynspeed.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-txtinfo/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/txtinfo/olsrd_txtinfo.so.* $(1)/usr/lib/
endef

define Package/olsrd-mod-watchdog/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/lib/watchdog/olsrd_watchdog.so.* $(1)/usr/lib/
endef

$(eval $(call BuildPackage,olsrd))
$(eval $(call BuildPackage,olsrd-mod-arprefresh))
$(eval $(call BuildPackage,olsrd-mod-dot-draw))
$(eval $(call BuildPackage,olsrd-mod-bmf))
$(eval $(call BuildPackage,olsrd-mod-dnssd))
$(eval $(call BuildPackage,olsrd-mod-dyn-gw))
$(eval $(call BuildPackage,olsrd-mod-dyn-gw-plain))
$(eval $(call BuildPackage,olsrd-mod-httpinfo))
$(eval $(call BuildPackage,olsrd-mod-jsoninfo))
$(eval $(call BuildPackage,olsrd-mod-mdns))
$(eval $(call BuildPackage,olsrd-mod-mdp))
$(eval $(call BuildPackage,olsrd-mod-nameservice))
$(eval $(call BuildPackage,olsrd-mod-p2pd))
$(eval $(call BuildPackage,olsrd-mod-pgraph))
$(eval $(call BuildPackage,olsrd-mod-quagga))
$(eval $(call BuildPackage,olsrd-mod-secure))
$(eval $(call BuildPackage,olsrd-mod-sgwdynspeed))
$(eval $(call BuildPackage,olsrd-mod-txtinfo))
$(eval $(call BuildPackage,olsrd-mod-watchdog))
