#
# Copyright (C) 2006-2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=zlib
PKG_VERSION:=1.2.12
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://zlib.net/
PKG_MD5SUM:=5fc414a9726be31427b440b434d05f78

HOST_FIXUP:=autoreconf

HOST_BUILD_PARALLEL:=1

include $(BUILD_DIR)/host-build.mk

HOST_CFLAGS += $(FPIC)

HOST_CONFIGURE_VARS += \
	CHOST=$(GNU_HOST_NAME) \
	LDSHARED="$(TARGET_CC) -shared -Wl,-soname,libz.so.1" \

HOST_CONFIGURE_ARGS = \
	--prefix=$(HOST_BUILD_PREFIX) \
	--eprefix=$(HOST_BUILD_PREFIX) \
	--sysconfdir=$(HOST_BUILD_PREFIX)/etc \
	--localstatedir=$(HOST_BUILD_PREFIX)/var

define Host/Configure
	$(if $(HOST_CONFIGURE_PARALLEL),+)(cd $(HOST_BUILD_DIR)/$(3); \
		if [ -x configure ]; then \
			$(CP) $(SCRIPT_DIR)/config.{guess,sub} $(HOST_BUILD_DIR)/$(3)/ && \
			$(2) \
			$(HOST_CONFIGURE_VARS) \
			$(HOST_CONFIGURE_CMD) \
			$(HOST_CONFIGURE_ARGS) \
			--enable-shared; \
		fi \
	)
endef

define Host/Install
	$(MAKE) -C $(HOST_BUILD_DIR) install
endef

$(eval $(call HostBuild))

