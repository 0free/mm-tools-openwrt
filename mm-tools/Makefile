
include $(TOPDIR)/rules.mk

PKG_NAME:=mm-tools
PKG_VERSION:=1.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.bz2
PKG_SOURCE_URL:=https://github.com/SaifAlSubhi/mm-tools.git
PKG_HASH:=a35d5b99332e0d152a33a47bf6d924165efda7f077980a7bcbd976c0312dd961

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)

PKG_FIXUP:=autoreconf
PKG_INSTALL:=1

include $(INCLUDE_DIR)/package.mk

define Package/mm-tools
	TITLE:=mm-tools
	SECTION:=utils
	CATEGORY:=Utilities
	DEPENDS:=+libpthread 
endef

define Package/mm-tools/description
mm-tools is a tool to reflash MM firmware for Avalon (supports AUC and IIC interface)
endef

define Package/mm-tools/config
	source "$(SOURCE)/Config.in"
endef

TARGET_CFLAGS += 
TARGET_LDFLAGS += -L$(STAGING_DIR)/usr/lib

ifeq ($(CONFIG_MM_TOOLS_SOFT_CRC),y)
	CONFIGURE_ARGS += --enable-soft-crc
endif

ifeq ($(CONFIG_MM_TOOLS_FLASH_LAYOUT_2M),y)
	CONFIGURE_ARGS += --enable-flash-layout-2m
endif

define Build/Configure
	$(call Build/Configure/mm-tools)
	(cd $(PKG_BUILD_DIR) && aclocal && libtoolize && autoreconf && automake;)
	(cd $(PKG_BUILD_DIR) && chmod +x configure && ./configure $(CONFIGURE_ARGS);)
endef

define Build/Compile
	$(call Build/Compile/mm-tools)
endef

define Package/mm-tools/install
  $(INSTALL_DIR)    $(1)/usr/bin
  $(CP)             $(PKG_BUILD_DIR)/mm-tools		$(1)/usr/bin
endef

$(eval $(call BuildPackage,mm-tools))
