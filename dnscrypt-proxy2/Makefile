#
# Copyright (C) 2019-2021 CZ.NIC, z. s. p. o. (https://www.nic.cz/)
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=dnscrypt-proxy2
PKG_VERSION:=2.1.12
PKG_RELEASE:=1

PKG_SOURCE:=dnscrypt-proxy-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/DNSCrypt/dnscrypt-proxy/tar.gz/$(PKG_VERSION)?
PKG_HASH:=95fe29ed03dad0cdd34d47316ecb15c25f7ef5fab21d9ec52cbfc4743bcf5198
PKG_BUILD_DIR:=$(BUILD_DIR)/dnscrypt-proxy-$(PKG_VERSION)

PKG_MAINTAINER:=Josef Schlehofer <pepe.schlehofer@gmail.com>
PKG_LICENSE:=ISC
PKG_LICENSE_FILES:=LICENSE

PKG_BUILD_DEPENDS:=golang/host
PKG_BUILD_PARALLEL:=1
PKG_BUILD_FLAGS:=no-mips16

GO_PKG:=github.com/DNSCrypt/dnscrypt-proxy

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/golang/golang-package.mk

GO_MOD_ARGS:=
GO_PKG_BUILD_VARS+= GO111MODULE=off

define Package/dnscrypt-proxy2
  SECTION:=net
  CATEGORY:=Network
  TITLE:=Flexible DNS proxy with encrypted DNS protocols
  URL:=https://github.com/DNSCrypt/dnscrypt-proxy
  DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
  CONFLICTS:=dnscrypt-proxy
endef

define Package/dnscrypt-proxy2/install
	$(call GoPackage/Package/Install/Bin,$(PKG_INSTALL_DIR))

	$(INSTALL_DIR) $(1)/usr/sbin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/* $(1)/usr/sbin/

	$(INSTALL_DIR) $(1)/etc/dnscrypt-proxy2
	$(INSTALL_CONF) $(PKG_BUILD_DIR)/dnscrypt-proxy/example-dnscrypt-proxy.toml $(1)/etc/dnscrypt-proxy2/dnscrypt-proxy.toml
	$(INSTALL_CONF) ./files/blocked-names.txt $(1)/etc/dnscrypt-proxy2/blocked-names.txt

	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/dnscrypt-proxy.init $(1)/etc/init.d/dnscrypt-proxy

	sed -i "s/^listen_addresses = .*/listen_addresses = ['127.0.0.53:53']/" $(1)/etc/dnscrypt-proxy2/dnscrypt-proxy.toml
	sed -i "s/^  # blocked_names_file = 'blocked-names.txt'/blocked_names_file = 'blocked-names.txt'/" $(1)/etc/dnscrypt-proxy2/dnscrypt-proxy.toml
endef

define Package/dnscrypt-proxy2/description
  A flexible DNS proxy, with support for modern encrypted DNS protocols
  such as DNSCrypt v2 and DNS-over-HTTPS.
endef

define Package/dnscrypt-proxy2/conffiles
/etc/dnscrypt-proxy2/dnscrypt-proxy.toml
endef

define Package/golang-github-jedisct1-dnscrypt-proxy2-dev
$(call Package/dnscrypt-proxy2)
$(call GoPackage/GoSubMenu)
  TITLE+= (source files)
  PKGARCH:=all
endef

define Package/golang-github-jedisct1-dnscrypt-proxy2-dev/description
$(call Package/dnscrypt-proxy2/description)

  This package provides the source files for the client/bridge program.
endef

$(eval $(call GoBinPackage,dnscrypt-proxy2))
$(eval $(call BuildPackage,dnscrypt-proxy2))
$(eval $(call GoSrcPackage,golang-github-jedisct1-dnscrypt-proxy2-dev))
$(eval $(call BuildPackage,golang-github-jedisct1-dnscrypt-proxy2-dev))
