# OpenWRT/LEDE Makefile of https://github.com/shadowsocksrr/shadowsocksr-libev

include $(TOPDIR)/rules.mk

PKG_NAME:=shadowsocksr-libev
PKG_VERSION:=2018-03-07
PKG_RELEASE:=4799b312b8244ec067b8ae9ba4b85c877858976c
PKG_MIRROR_HASH:=59ffe5c0da9f467bd816bc9d27560f987c6b1a54c74b160d3800328fd190b8af

PKG_SOURCE_PROTO:=git
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE).tar.gz
PKG_SOURCE_URL:=https://github.com/shadowsocksrr/shadowsocksr-libev.git
PKG_SOURCE_VERSION:=$(PKG_RELEASE)
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE)

PKG_LICENSE:=GPLv3
PKG_LICENSE_FILES:=LICENSE
PKG_MAINTAINER:=chenhw2 <https://github.com/chenhw2>

PKG_BUILD_DIR:=$(BUILD_DIR)/$(PKG_NAME)/$(BUILD_VARIANT)/$(PKG_NAME)-$(PKG_VERSION)-$(PKG_RELEASE)

PKG_INSTALL:=1
PKG_FIXUP:=autoreconf
PKG_USE_MIPS16:=0
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/shadowsocksr-libev/Default
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Lightweight Secured Socks5 Proxy
	URL:=https://github.com/shadowsocksrr/shadowsocksr-libev
	DEPENDS:=+libev +libsodium +libopenssl +libpthread +libpcre2 +libudns +zlib +libopenssl-legacy
endef

Package/shadowsocksr-libev = $(call Package/shadowsocksr-libev/Default)

define Package/shadowsocksr-libev/description
shadowsocksr-libev is a lightweight secured socks5 proxy for embedded devices and low end boxes.
endef

CONFIGURE_ARGS += --disable-ssp --disable-documentation --disable-assert

define Package/shadowsocksr-libev/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ss-local $(1)/usr/bin/ssr-local
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ss-redir $(1)/usr/bin/ssr-redir
	$(LN) ssr-local $(1)/usr/bin/ssr-tunnel
endef

$(eval $(call BuildPackage,shadowsocksr-libev))
