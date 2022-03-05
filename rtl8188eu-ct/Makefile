include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8188eu-ct
PKG_RELEASE=2

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/lwfinger/rtl8188eu.git
# PKG_MIRROR_HASH:=fa689e034cad9e4683ea784b8f3cb590492ab5c68e8babd492a4e8bf2de3b114
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2022-02-03
PKG_SOURCE_VERSION:=775137f273ed75bd092b281470accb827a3e1355

PKG_MAINTAINER:Larry Finger <lwfinger@github.com>
PKG_BUILD_PARALLEL:=1
#PKG_EXTMOD_SUBDIRS:=rtl8188eu-ct

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8188eu-ct
  SUBMENU:=Wireless Drivers
  TITLE:=Driver for Realtek 8188 EU devices TP-Link D-Link, etc
  DEPENDS:=+kmod-cfg80211 +kmod-usb-core +@DRIVER_11N_SUPPORT +@DRIVER_11AC_SUPPORT
  FILES:=\
	$(PKG_BUILD_DIR)/8188eu.ko
  AUTOLOAD:=$(call AutoProbe,8188eu)
  PROVIDES:=kmod-rtl8188eu
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

NOSTDINC_FLAGS+= -DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		modules
endef

$(eval $(call KernelPackage,rtl8188eu-ct))
