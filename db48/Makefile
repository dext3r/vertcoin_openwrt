#
# Copyright (C) 2009-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/uclibc++.mk

BASE_VERSION:=4.8.30

PKG_NAME:=db48
PKG_VERSION:=$(BASE_VERSION).4.NC
PKG_RELEASE:=5

PKG_BUILD_DIR:=$(BUILD_DIR)/db-$(BASE_VERSION).NC
PKG_SOURCE:=db-$(BASE_VERSION).NC.tar.gz
PKG_SOURCE_URL:=http://download.oracle.com/berkeley-db/
PKG_MD5SUM:=a14a5486d6b4891d2434039a0ed4c5b7
#PKG_MD5SUM:=073ab7f20d24b3872a51ca762f5090e7
#http://download.oracle.com/berkeley-db/db-4.8.30.NC.tar.gz


PKG_MAINTAINER:=dext0rb
PKG_LICENSE:=poops
PKG_LICENSE_FILES:=LICENSE

PKG_FIXUP:=autoreconf
PKG_LIBTOOL_PATHS:=. build_unix
PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define Package/libdb48
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libxml2
  TITLE:=Berkeley DB library (4.8)
  URL:=http://www.oracle.com/us/products/database/berkeley-db
  PROVIDES:=libdb48-full
endef

define Package/libdb48/description
  Berkeley DB library (4.8).
endef

define Package/libdb48xx
  SECTION:=libs
  CATEGORY:=Libraries
  DEPENDS:=+libdb48 $(CXX_DEPENDS)
  TITLE:=Berkeley DB library (4.8) for C++
  URL:=http://www.oracle.com/us/products/database/berkeley-db
  PROVIDES:=libdb48xx-full
endef

define Package/libdb48xx/description
  Berkeley DB library (4.8).  C++ wrapper.
endef

CONFIGURE_PATH = build_unix
CONFIGURE_CMD = ../dist/configure

CONFIGURE_ARGS += \
	--enable-shared \
	--enable-static \
	--disable-java \
	--with-mutex=POSIX/pthreads/library \
	--disable-tcl \
	--enable-compat185 \
	--disable-debug \
	$(if $(CONFIG_PACKAGE_libdb48xx),--enable-cxx,--disable-cxx)

TARGET_CFLAGS += $(FPIC)

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C $(PKG_BUILD_DIR)/build_unix \
		DESTDIR="$(PKG_INSTALL_DIR)" all
	$(MAKE) -C $(PKG_BUILD_DIR)/build_unix \
		DESTDIR="$(PKG_INSTALL_DIR)" install
endef

define Package/libdb48/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb-*.so $(1)/usr/lib/
endef

define Package/libdb48xx/install
	$(INSTALL_DIR) $(1)/usr/lib
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb_cxx-*.so $(1)/usr/lib/
endef

define Build/InstallDev
	$(INSTALL_DIR) $(1)/usr/include
	$(CP) $(PKG_INSTALL_DIR)/usr/include/db.h $(1)/usr/include/
	$(CP) $(PKG_INSTALL_DIR)/usr/include/db_cxx.h $(1)/usr/include/
	$(INSTALL_DIR) $(1)/usr/lib/
	$(CP) $(PKG_INSTALL_DIR)/usr/lib/libdb*.{a,so} $(1)/usr/lib
endef

$(eval $(call BuildPackage,libdb48))
$(eval $(call BuildPackage,libdb48xx))
