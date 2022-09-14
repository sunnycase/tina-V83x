#
# Copyright (C) 2006-2015 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

PKG_NAME:=mtd-utils
PKG_VERSION:=2.1.4

PKG_SOURCE_VERSION:=c7f1bfa44a84d02061787e2f6093df5cc40b9f5c
PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION)-$(PKG_SOURCE_VERSION).tar.gz
PKG_SOURCE_URL:=git://git.infradead.org/mtd-utils.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)-$(PKG_VERSION)
PKG_MIRROR_MD5SUM:=
PKG_CAT:=zcat

HOST_BUILD_DIR:=$(COMPILE_DIR_HOST)/$(PKG_NAME)-$(PKG_VERSION)

include $(BUILD_DIR)/host-build.mk

HOST_CONFIGURE_ARGS += \
    --without-xattr \
	--without-lzo

define Host/Configure
	$(if $(HOST_CONFIGURE_PARALLEL),+)(cd $(HOST_BUILD_DIR)/$(3); \
		$(BASH) ./autogen.sh && \
		if [ -x configure ]; then \
			$(CP) $(SCRIPT_DIR)/config.{guess,sub} $(HOST_BUILD_DIR)/$(3)/ && \
			$(2) \
			$(HOST_CONFIGURE_CMD) \
			$(HOST_CONFIGURE_VARS) \
			$(HOST_CONFIGURE_ARGS) \
			$(1); \
		fi \
	)
endef

HOST_CFLAGS += -I$(STAGING_DIR_HOST)/include/e2fsprogs -I$(STAGING_DIR_HOST)/include/e2fsprogs/uuid
CFLAGS := $(HOST_CFLAGS) -I$(HOST_BUILD_DIR)/include -L$(HOST_BUILD_DIR) -L$(STAGING_DIR_HOST)/lib -DNO_NATIVE_SUPPORT
ifneq ($(HOST_OS),Linux)
CFLAGS += -Dloff_t=off_t -D__BYTE_ORDER=BYTE_ORDER -include getline.h -include endian.h -I$(CURDIR)/include -include fls.h
MTD_STATIC := 0
else
MTD_STATIC := 1
endif

MTD_MAKEOPTS = \
	CFLAGS="$(CFLAGS)" \
	LDFLAGS="$(HOST_LDFLAGS)" \
	WITHOUT_LZO=1 WITHOUT_XATTR=1 \
	SUBDIRS="" \
	BUILDDIR="$(HOST_BUILD_DIR)" \
	STATIC=$(MTD_STATIC) \
	V=$(if $(findstring c,$(OPENWRT_VERBOSE)),1)

define Host/Compile
	$(MAKE) -C $(HOST_BUILD_DIR) \
		$(MTD_MAKEOPTS) \
		TARGETS="mkfs.jffs2 ubi-utils/ubinize mkfs.ubifs/mkfs.ubifs"
endef

define Host/Install
	$(CP) \
		$(HOST_BUILD_DIR)/mkfs.jffs2 \
		$(HOST_BUILD_DIR)/mkfs.ubifs \
		$(HOST_BUILD_DIR)/ubinize \
		$(STAGING_DIR_HOST)/bin/
endef

define Host/Clean
	rm -f $(STAGING_DIR_HOST)/bin/{mkfs.jffs2,mkfs.ubifs,ubinize}
endef

$(eval $(call HostBuild))
