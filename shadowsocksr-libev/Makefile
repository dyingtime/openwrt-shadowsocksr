# OpenWRT/LEDE Makefile of https://github.com/shadowsocksrr/shadowsocksr-libev

include $(TOPDIR)/rules.mk

PKG_NAME:=shadowsocksr-libev
PKG_VERSION:=2018-03-07
PKG_RELEASE:=d63ff863800a5645aca4309d5dd5962bd1e95543
PKG_MIRROR_HASH:=9f90a8ac4fd57de1b8f5aae09e8e2da81f4cf387e487710aec5e34c4af98d740

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
PKG_BUILD_DEPENDS:=mbedtls pcre

PKG_CONFIG_DEPENDS:= \
	CONFIG_SHADOWSOCKSR_STATIC_LINK \
	CONFIG_SHADOWSOCKSR_WITH_PCRE \
	CONFIG_SHADOWSOCKSR_WITH_MBEDTLS

include $(INCLUDE_DIR)/package.mk

define Package/shadowsocksr-libev/Default
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Lightweight Secured Socks5 Proxy
	URL:=https://github.com/shadowsocksrr/shadowsocksr-libev
	DEPENDS:=+zlib +libpthread +libsodium
endef

Package/shadowsocksr-libev = $(call Package/shadowsocksr-libev/Default)

define Package/shadowsocksr-libev/config
menu "Shadowsocksr-libev Compile Configuration"
	depends on PACKAGE_shadowsocksr-libev
	config SHADOWSOCKSR_STATIC_LINK
		bool "enable static link libraries."
		default n

		menu "Select libraries"
			depends on SHADOWSOCKSR_STATIC_LINK
			config SHADOWSOCKSR_WITH_PCRE
				bool "static link libpcre."
				default n
			config SHADOWSOCKSR_WITH_MBEDTLS
				bool "static link libmbedtls."
				default n
		endmenu
endmenu
endef

define Package/shadowsocksr-libev/description
shadowsocksr-libev is a lightweight secured socks5 proxy for embedded devices and low end boxes.
endef

# 配置系统 libsodium 的路径
CONFIGURE_VARS += \
	LIBSODIUM_CFLAGS="-I$(STAGING_DIR)/usr/include" \
	LIBSODIUM_LIBS="-lsodium"

# 删除源码中自带的旧 libsodium，强制使用系统版本
define Build/Prepare
	$(call Build/Prepare/Default)
	rm -rf $(PKG_BUILD_DIR)/libsodium
endef

# 添加编译器 include 和 lib 路径
TARGET_CFLAGS += -I$(STAGING_DIR)/usr/include
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib

CONFIGURE_ARGS += --disable-ssp --disable-documentation --disable-assert --with-crypto-library=mbedtls USE_SYSTEM_SHARED_LIB=1

define Package/shadowsocksr-libev/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ss-local $(1)/usr/bin/ssr-local
	$(INSTALL_BIN) $(PKG_BUILD_DIR)/src/ss-redir $(1)/usr/bin/ssr-redir
	$(LN) ssr-local $(1)/usr/bin/ssr-tunnel
endef

$(eval $(call BuildPackage,shadowsocksr-libev))
