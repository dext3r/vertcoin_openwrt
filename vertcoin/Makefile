# Vertcoin for OpenWRT
# --------------------
# Packaged by: dext0rb@gmail.com
#
# The Vertcoin project is volunteer driven and relies on your support.
# Please consider donating your skills or VTC to the Vertcoin project.
#
# Developer Fund: VpBsRnN749jYHE9hT8dZreznHfmFMdE1yG
#
# Marketing Fund: VqQWx5p7Yr6ns4dPNQEM6j3JvA9rdBEDm4
#
#
# Based on the work of Jimmy Situ <web@jimmystone.cn>
# BTC Address: 1KHujLT4AzQwQKSLEUSbcergqv7fMnQNXA
#
# This is free and unencumbered software released into the public domain.
# For details see the UNLICENSE file at the root of the source tree.
include $(TOPDIR)/rules.mk

PKG_NAME:=vertcoin
#PKG_VERSION:= 0.13.0
#PKG_REV:=1a4381eb29157c05b938a2db6d8e31cb56819ea4
PKG_VERSION:=0.13.2
PKG_REV:=bc600348443646eb4c5754619e48eb4c35132f0b
PKG_RELEASE:=1
PKG_INSTALL:=1
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_REV).tar.bz2
PKG_SOURCE_URL:=https://github.com/vertcoin-project/vertcoin-core.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_SOURCE_VERSION:=$(PKG_REV)
PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/$(PKG_NAME)
	SECTION:=net
	CATEGORY:=Network
	TITLE:=Vertcoin daemon and CLI
	URL:=https://github.com/vertcoin-project/
	DEPENDS := +libcurl +libpthread +jansson +libncurses
	DEPENDS += +boost-chrono +boost-filesystem +boost-program_options +boost-thread +boost-test
	DEPENDS += +libopenssl +libssp +libstdcpp
	DEPENDS += +libatomic +libevent2 +libevent2-pthreads
ifeq ($(CONFIG_VERTCOIN_ENABLE_WALLET),y)
	DEPENDS += +libdb48 +libdb48xx
endif
endef

define Package/$(PKG_NAME)/description
Vertcoin is a P2P decentralized cryptocurrency
endef

define Package/$(PKG_NAME)/config
	menu "Configuration"
	depends on PACKAGE_vertcoin
	source "$(SOURCE)/Config.in"
	endmenu
endef

TARGET_CFLAGS  += -std=c99
TARGET_CXXFLAGS += -std=c++11
TARGET_LDFLAGS += -Wl,-rpath-link=$(STAGING_DIR)/usr/lib

ifeq ($(CONFIG_VERTCOIN),y)
	CONFIGURE_ARGS += --without-miniupnpc
 	CONFIGURE_ARGS += --with-boost-libdir=$(STAGING_DIR)/usr/lib
ifeq ($(CONFIG_VERTCOIN_ENABLE_WALLET), n)
echo "we should be disabling wallet!!!!!!!!\n"
CONFIGURE_ARGS += --disable-wallet
endif
endif

define Build/Compile
	$(call Build/Compile/Default)
endef

define Package/$(PKG_NAME)/install

# Make configuration
	$(INSTALL_DIR) $(1)/root/.vertcoin
	echo "server=1" > $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "daemon=1" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "rpcuser=vtc" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "rpcpassword=supersecret" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "maxconnections=5" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "maxuploadtarget=5000" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "maxmempool=50" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "maxorphantx=10" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "dbcache=100" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	echo "rpcthreads=1" >> $(PKG_INSTALL_DIR)/vertcoin.conf
	$(INSTALL_DATA) $(PKG_INSTALL_DIR)/vertcoin.conf $(1)/root/.vertcoin

# Alternate install options allow for:
#	* installation on USB flash
# 	* side-loading block db
ifeq ($(CONFIG_VERTCOIN_ALT_INSTALL),y)
ifneq ($(CONFIG_VERTCOIN_ALT_INSTALL_DIR), '')
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vertcoind	$(CONFIG_VERTCOIN_ALT_INSTALL_DIR)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vertcoin-cli	$(CONFIG_VERTCOIN_ALT_INSTALL_DIR)
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vertcoin-tx	$(CONFIG_VERTCOIN_ALT_INSTALL_DIR)
endif

ifeq ($(CONFIG_VERTCOIN_COPY_BLOCK_DB),y)
ifneq ($(CONFIG_VERTCOIN_COPY_BLOCK_DB_DIR), '')
	rsync -r --info=progress2 $(CONFIG_VERTCOIN_COPY_BLOCK_DB_DIR)/blocks $(CONFIG_VERTCOIN_ALT_INSTALL_DIR)
	rsync -r --info=progress2 $(CONFIG_VERTCOIN_COPY_BLOCK_DB_DIR)/chainstate	$(CONFIG_VERTCOIN_ALT_INSTALL_DIR)
endif
endif
else
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_DIR) $(1)/etc/config

	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vertcoind	$(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vertcoin-cli	$(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/usr/bin/vertcoin-tx	$(1)/usr/bin
endif
# TODO : Fancy service stuff
#		Should install vertcoind as a service
# TODO : Configuration defaults
#		Generate a configuration file - copy it to /root/.vertcoin
# TODO : Wallet, ZeroMQ, etc menu options
#		Add packages/change menu options Makefile
endef

$(eval $(call BuildPackage,$(PKG_NAME)))
