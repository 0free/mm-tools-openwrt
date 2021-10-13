
include $(TOPDIR)/rules.mk

PKG_NAME:=mm-cgminer
PKG_REV:=main
PKG_VERSION:=1
PKG_RELEASE:=1
PKG_INSTALL:=1

PKG_SOURCE_URL:=https://github.com/SaifAlSubhi/mm-cgminer.git
PKG_SOURCE:=$(PKG_NAME).tar.bz2
PKG_HASH:=a35d5b99332e0d152a33a47bf6d924165efda7f077980a7bcbd976c0312dd961

PKG_SOURCE_PROTO:=git
PKG_SOURCE_SUBDIR:=$(PKG_NAME)
PKG_SOURCE_VERSION:=$(PKG_REV)

PKG_FIXUP:=autoreconf

include $(INCLUDE_DIR)/package.mk

define Package/mm-cgminer/Default
	SECTION:=utils
	CATEGORY:=Utilities
	TITLE:=mm-cgminer
	URL:=
	DEPENDS:=+libpthread +udev
endef

define Package/mm-cgminer/description
mm-cgminer is a tool to reflash MM firmware for Avalon
It supports AUC and IIC interface
endef

define Package/mm-cgminer/config
	menu "Configuration"
	source "$(SOURCE)/Config.in"
	endmenu
endef

define Package/mm-cgminer
	$(call Package/mm-cgminer/Default)
	TITLE+=(for Avalon)
endef

ifeq ($(CONFIG_MM_CGMINER_SOFT_CRC),y)
	CONFIGURE_ARGS += --enable-soft-crc
endif

ifeq ($(CONFIG_MM_CGMINER_FLASH_LAYOUT_2M),y)
	CONFIGURE_ARGS += --enable-flash-layout-2m
endif

define Build/Install
	$(call Build/Compile/Default)
endef

define Package/mm-cgminer/install
	$(INSTALL_DIR) $(1)/usr/bin
	cp $(PKG_BUILD_DIR)/$(PKG_NAME) $(1)/usr/bin
endef

$(eval $(call BuildPackage,mm-cgminer))
