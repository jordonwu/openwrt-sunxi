#
# Copyright (C) 2010-2011 OpenWrt.org
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#
include $(TOPDIR)/rules.mk

ARCH:=arm
BOARD:=sunxi
BOARDNAME:=Allwinner A10
FEATURES:=fpu usb ext4 display
SUBTARGETS=cubie
# 
#CFLAGS:=-O3 -pipe -fomit-frame-pointer -mfloat-abi=softfp -mfpu=vfp -mcpu=arm1176jzf-s
CFLAGS:=-O2 -pipe -march=armv7-a -mfloat-abi=hard -mfpu=neon \
	-ftree-vectorize -mvectorize-with-neon-quad -mcpu=cortex-a8 -mtune=cortex-a8 \
	-ffast-math

MAINTAINER:=Andrew Andrianov <andrew@ncrmnt.org>

LINUX_VERSION:=3.7-rc7

include $(INCLUDE_DIR)/target.mk

define Target/Description
	Images for Allwinner A10 based boards
endef

KERNELNAME:="uImage"

$(eval $(call BuildTarget))
