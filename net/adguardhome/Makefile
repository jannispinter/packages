#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=adguardhome
PKG_VERSION:=0.103.3
PKG_RELEASE:=1

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=v$(PKG_VERSION)
PKG_SOURCE_URL:=https://github.com/AdguardTeam/AdGuardHome
PKG_MIRROR_HASH:=edf00ede9d208b7f78574e267ad2617dd4d4f06548cb2e6369cc1ad702427d9a

PKG_LICENSE:=GPL-3.0-only
PKG_LICENSE_FILES:=LICENSE.txt
PKG_MAINTAINER:=Dobroslaw Kijowski <dobo90@gmail.com>

PKG_BUILD_DEPENDS:=golang/host node/host packr/host
PKG_BUILD_PARALLEL:=1
PKG_USE_MIPS16:=0

GO_PKG:=github.com/AdguardTeam/AdGuardHome
GO_PKG_EXCLUDES:=dhcpd/standalone
GO_PKG_LDFLAGS_X:=main.version=$(PKG_VERSION) main.channel=release main.goarm=$(GO_ARM)

include $(INCLUDE_DIR)/package.mk
include ../../lang/golang/golang-package.mk

define Package/adguardhome
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Network-wide ads and trackers blocking DNS server
	URL:=https://github.com/AdguardTeam/AdGuardHome
	DEPENDS:=$(GO_ARCH_DEPENDS) +ca-bundle
endef

define Package/adguardhome/conffiles
/etc/adguardhome.yaml
endef

define Package/adguardhome/description
Free and open source, powerful network-wide ads and trackers blocking DNS server.
endef

define Build/Compile
	( \
		pushd $(PKG_BUILD_DIR) ; \
		npm --prefix client ci ; \
		npm --prefix client run build-prod ; \
		packr -z -v -i home ; \
		popd ; \
		$(call GoPackage/Build/Compile) ; \
	)
endef

define Package/adguardhome/install
	$(call GoPackage/Package/Install/Bin,$(1))
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/adguardhome.init $(1)/etc/init.d/adguardhome
endef

$(eval $(call GoBinPackage,adguardhome))
$(eval $(call BuildPackage,adguardhome))
