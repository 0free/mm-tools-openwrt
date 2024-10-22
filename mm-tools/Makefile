
include $(TOPDIR)/rules.mk

PKG_NAME:=mm-tools
PKG_VERSION:=1.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME).tar.bz2
PKG_SOURCE_URL:=https://github.com/SaifAlSubhi/mm-tools.git
PKG_HASH:=6261dd7c6582804f04b1f7d075c1e4cfcdc05655dc37ce3164dd0d86764fa0fd
PKG_MIRROR_HASH:=34e8b179b42b041aefd3663c628d6427be8d829fd7f8d3a4c7c9ee2af00454b6

PKG_SOURCE_PROTO:=git
PKG_SOURCE_VERSION:=$(PKG_VERSION)

include $(INCLUDE_DIR)/package.mk

define Package/mm-tools
	TITLE:=mm-tools
	SECTION:=utils
	CATEGORY:=Utilities
	DEPENDS:= +libpthread +udev +jansson
endef

define Package/mm-tools/description
mm-tools is a tool to reflash MM firmware for Avalon (supports AUC and IIC interface)
endef

define Package/mm-tools/config
	source "$(SOURCE)/Config.in"
endef

TARGET_LDFLAGS += $(STAGING_DIR)/usr/lib

mm_tools_CFLAGS = $(TARGET_CC) -fcommon
mm_tools_CFLAGS += -ljansson -lusb-1.0
mm_tools_CFLAGS += -I$(PKG_BUILD_DIR)/compat/jansson-2.13.1/src
mm_tools_CFLAGS += -I$(PKG_BUILD_DIR)/compat/libusb-1.0/libusb

mm_tools_SOURCES = main.c
mm_tools_SOURCES += api.h auc.h crc.h iic.h auc.c mboot_api.c mm_api.c iic_rpi.c iic_auc.c

ifeq ($(CONFIG_MM_TOOLS_SOFT_CRC),y)
	mm_tools_SOURCES += crc16_soft.c
else
	mm_tools_SOURCES += crc16.c
endif

ifeq ($(CONFIG_MM_TOOLS_FLASH_LAYOUT_2M),y)
	mm_tools_CPPFLAGS += -DUSE_FLASH_LAYOUT_2M
endif

define Build/Compile
	$(call Build/Compile/mm-tools)
	(cd $(PKG_BUILD_DIR) && $(mm_tools_CFLAGS) \
	`pkg-config --cflags libusb-1.0` \
	$(mm_tools_SOURCES) -o mm-tools \
	;)
endef

define Package/mm-tools/install

	$(INSTALL_DIR)		$(1)/usr/bin
	$(INSTALL_DIR)		$(1)/usr/lib

	$(CP)		$(PKG_BUILD_DIR)/mm-tools						$(1)/usr/bin
#	$(CP)		$(STAGING_DIR)/usr/lib/libjansson.so.*			$(1)/usr/lib
#	$(CP)		$(STAGING_DIR)/usr/lib/libusb-0.1.so.*			$(1)/usr/lib
#	$(CP)		$(STAGING_DIR)/usr/lib/libusb-1.0.so.*			$(1)/usr/lib
#	$(CP)		$(STAGING_DIR)/usr/lib/libudev.so.*				$(1)/usr/lib

endef

$(eval $(call BuildPackage,mm-tools))
