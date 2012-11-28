#
# Copyright (C) 2012 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk
include $(INCLUDE_DIR)/image.mk

define Image/Prepare
	cp $(LINUX_DIR)/arch/arm/boot/uImage $(KDIR)/uImage
endef

define Image/BuildKernel
	#TODO: Figure what this shit does
	#mkimage -A arm -O linux -T script -C none -a 0 -e 0 -n 'Boot Image' -d boot.script $(BIN_DIR)/boot.scr
	cp $(KDIR)/uImage $(BIN_DIR)/openwrt-$(BOARD)-uImage
endef

JFFS2_BLOCKSIZE=128k
JFFS2OPTS += --little-endian --pagesize=0x800 --no-cleanmarkers --pad
UBIFS_OPTS = -m 2048 -e 126KiB -c 1996 -U
UBINIZE_OPTS = -m 2048 -p 128KiB -s 512

offset=12288
define Image/Build/Pack/k1879
	dd if=/dev/zero of=$(BIN_DIR)/openwrt-$(BOARD)-$(1)-sysupgrade.bin bs=1024 count=15000
	dd if=$(BIN_DIR)/openwrt-$(BOARD)-uImage of=$(BIN_DIR)/openwrt-$(BOARD)-$(1)-sysupgrade.bin conv=notrunc
	dd if=$(BIN_DIR)/openwrt-$(BOARD)-$(1).img bs=1024 seek=$(offset) of=$(BIN_DIR)/openwrt-$(BOARD)-$(1)-sysupgrade.bin
endef

define Image/Build/Pack/dtb
	dd if=$(BIN_DIR)/openwrt-$(BOARD)-uImage of=$(BIN_DIR)/openwrt-$(BOARD)-$(1)-sysupgrade.bin
	dd if=$(BIN_DIR)/openwrt-$(BOARD)-$(1).img bs=1024 seek=$(offset) conv=notrunc of=$(BIN_DIR)/openwrt-$(BOARD)-$(1)-sysupgrade.bin
endef


define Image/Build
	$(call Image/Build/$(1),$(1))
endef

define Image/Build/jffs2-64k
	dd if=$(KDIR)/root.$(1) of=$(BIN_DIR)/openwrt-$(BOARD)-$(1).img bs=65536 conv=sync
	$(call Image/Build/Pack/$(BOARD),$(1))
endef

define Image/Build/jffs2-128k
	dd if=$(KDIR)/root.$(1) of=$(BIN_DIR)/openwrt-$(BOARD)-$(1).img bs=131072 conv=sync
	$(call Image/Build/Pack/$(BOARD),$(1))
endef

define Image/Build/squashfs
	$(call prepare_generic_squashfs,$(KDIR)/root.squashfs)
	dd if=$(KDIR)/root.$(1) of=$(BIN_DIR)/openwrt-$(BOARD)-$(1).img bs=131072 conv=sync
	$(call Image/Build/Pack/$(BOARD),$(1))
endef

$(eval $(call BuildImage))
