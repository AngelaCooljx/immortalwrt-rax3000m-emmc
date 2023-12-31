# SPDX-License-Identifier: GPL-2.0-only
#
# Copyright (C) 2023 ImmortalWrt.org

include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/kernel.mk

PKG_NAME:=ice
PKG_VERSION:=1.12.6
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=@SF/e1000/ice%20stable/$(PKG_VERSION)/
PKG_HASH:=55a7fb96c516c178180e07451beb621997148301f1b1be0bc52bf970850bc5cc

PKG_MAINTAINER:=Tianling Shen <cnsztl@immortalwrt.org>
PKG_LICENSE:=GPL-2.0-only
PKG_LICENSE_FILES:=COPYING

PKG_BUILD_PARALLEL:=1

include $(INCLUDE_DIR)/package.mk

define KernelPackage/ice-vendor
  SUBMENU:=Network Devices
  TITLE:=Intel(R) Ethernet Controller E810 Family support (vendor driver)
  VERSION:=$(LINUX_VERSION)+$(PKG_VERSION)-$(BOARD)-$(PKG_RELEASE)
  DEPENDS:=@PCI_SUPPORT +kmod-ptp @TARGET_x86
  CONFLICTS:=kmod-ice
  FILES:=$(PKG_BUILD_DIR)/src/ice.ko
  AUTOLOAD:=$(call AutoProbe,ice)
endef

define KernelPackage/ice-vendor/description
  Kernel modules for Intel(R) Ethernet Controller E810 Series
endef

define Build/Configure
	( \
		pushd $(PKG_BUILD_DIR)/src ; \
		KSRC=$(LINUX_DIR) \
		OUT=kcompat_generated_defs.h \
		CONFFILE=$(LINUX_DIR)/include/generated/autoconf.h \
		bash kcompat-generator.sh ; \
		popd ; \
	)
endef

define Build/Compile
	+$(KERNEL_MAKE) $(PKG_JOBS) \
		M=$(PKG_BUILD_DIR)/src \
		modules
endef

$(eval $(call KernelPackage,ice-vendor))
