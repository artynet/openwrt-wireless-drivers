include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8192eu-ct-mange
PKG_RELEASE=2

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/Mange/rtl8192eu-linux-driver.git
# PKG_MIRROR_HASH:=fa689e034cad9e4683ea784b8f3cb590492ab5c68e8babd492a4e8bf2de3b114
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2021-10-23
PKG_SOURCE_VERSION:=744bbe52976e51895fce2c1d4075f97a98dca2b2

PKG_MAINTAINER:=Magnus Bergmark <mange@devbox.com>
PKG_BUILD_PARALLEL:=1
#PKG_EXTMOD_SUBDIRS:=rtl8192eu-ct

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8192eu-ct-mange
  SUBMENU:=Wireless Drivers
  TITLE:=Driver for Realtek 8192 AU devices comfast 912-ac, etc
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
  FILES:=\
	$(PKG_BUILD_DIR)/8192eu.ko
  AUTOLOAD:=$(call AutoProbe,8192eu)
  PROVIDES:=kmod-rtl8192eu
endef

NOSTDINC_FLAGS := \
	$(KERNEL_NOSTDINC_FLAGS) \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/backport.h

NOSTDINC_FLAGS+= -DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

$(eval $(call KernelPackage,rtl8192eu-ct-mange))
