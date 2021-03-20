include $(TOPDIR)/rules.mk

PKG_NAME:=rtl8723bs
PKG_RELEASE=1

PKG_LICENSE:=GPLv2
PKG_LICENSE_FILES:=

PKG_SOURCE_URL:=https://github.com/lyh802/rtl8723BS_WiFi_linux.git
PKG_SOURCE_PROTO:=git
PKG_SOURCE_DATE:=2021-07-15
PKG_SOURCE_VERSION:=04a86318c38244d963af5ad327797d748c64ba35
PKG_MIRROR_HASH:=8c5f2e28ce337545782526a19e9e75788821a6e049b9a4b04759228431f62988

PKG_MAINTAINER:=Hauke Mehrtens <hauke@hauke-m.de>
PKG_BUILD_PARALLEL:=1
#PKG_EXTMOD_SUBDIRS:=rtl8723bs

STAMP_CONFIGURED_DEPENDS := $(STAGING_DIR)/usr/include/mac80211-backport/backport/autoconf.h

include $(INCLUDE_DIR)/kernel.mk
include $(INCLUDE_DIR)/package.mk

define KernelPackage/rtl8723bs
  SUBMENU:=Wireless Drivers
  TITLE:=Realtek RTL8723BS SDIO WiFi
  DEPENDS:=+kmod-cfg80211 +kmod-mmc +@DRIVER_11N_SUPPORT 
  FILES:=$(PKG_BUILD_DIR)/8723bs.ko
#  AUTOLOAD:=$(call AutoProbe,rtl8723bs)
  AUTOLOAD:=$(call AutoLoad,50,8723bs)
endef

NOSTDINC_FLAGS = \
	-I$(PKG_BUILD_DIR) \
	-I$(PKG_BUILD_DIR)/include \
	-I$(STAGING_DIR)/usr/include/mac80211-backport \
	-I$(STAGING_DIR)/usr/include/mac80211-backport/uapi \
	-I$(STAGING_DIR)/usr/include/mac80211 \
	-I$(STAGING_DIR)/usr/include/mac80211/uapi \
	-include backport/backport.h

NOSTDINC_FLAGS+=-DCONFIG_LITTLE_ENDIAN
#NOSTDINC_FLAGS+=-DCONFIG_CONCURRENT_MODE
NOSTDINC_FLAGS+=-DCONFIG_IOCTL_CFG80211 -DRTW_USE_CFG80211_STA_EVENT -DBUILD_OPENWRT

define Build/Compile
	+$(MAKE) $(PKG_JOBS) -C "$(LINUX_DIR)" \
		$(KERNEL_MAKE_FLAGS) \
		M="$(PKG_BUILD_DIR)" \
		NOSTDINC_FLAGS="$(NOSTDINC_FLAGS)" \
		CONFIG_RTL8723BS=m \
		modules
endef

$(eval $(call KernelPackage,rtl8723bs))
