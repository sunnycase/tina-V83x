#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=zstd
PKG_VERSION:=1.5.2
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://github.com/facebook/zstd/
PKG_MD5SUM:=072b10f71f5820c24761a65f31f43e73

HOST_FIXUP:=autoreconf

HOST_BUILD_PARALLEL:=1

include $(BUILD_DIR)/host-build.mk

HOST_MAKE_FLAGS = \
	CC="$(HOSTCC)" \
	CFLAGS="$(HOST_CFLAGS) $(FPIC)" \
	CPPFLAGS="$(HOST_CPPFLAGS)" \
	SHELL="$(SHELL)"

define Host/Compile
	$(MAKE) $(HOST_JOBS) -C $(HOST_BUILD_DIR) \
		$(HOST_MAKE_FLAGS)
endef

define Host/Install
	$(MAKE) -C $(HOST_BUILD_DIR) install \
		PREFIX=$(HOST_BUILD_PREFIX) \
		EXEC_PREFIX=$(HOST_BUILD_PREFIX) \
		SYSCONFDIR=$(HOST_BUILD_PREFIX)/etc \
		LOCALSTATEDIR=$(HOST_BUILD_PREFIX)/var \
		SBINDIR=$(HOST_BUILD_PREFIX)/bin
endef

$(eval $(call HostBuild))

