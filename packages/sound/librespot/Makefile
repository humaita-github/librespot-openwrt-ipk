# SPDX-License-Identifier: GPL-3.0-only
#
# Copyright (C) 2023 Facundo Acevedo

include $(TOPDIR)/rules.mk

PKG_NAME:=librespot
PKG_VERSION:=0.7.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/librespot-org/librespot/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=61f90b661ba883890a7ed8fe3926372b99252abad11dcacf94f2c89df21b7746

PKG_MAINTAINER:=Leonardo
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=LICENCE

PKG_BUILD_DEPENDS:=rust/host
PKG_BUILD_PARALLEL:=1

RUST_PKG_FEATURES:=rustls-tls-webpki-roots alsa-backend with-libmdns

include $(INCLUDE_DIR)/package.mk
include $(TOPDIR)/feeds/packages/lang/rust/rust-package.mk

define Package/librespot
  SECTION:=sound
  CATEGORY:=Sound
  TITLE:=Spotify client
  DEPENDS:= +alsa-lib +libavahi-client
  URL:=https://github.com/librespot-org/librespot
endef

define Package/librespot/description
  librespot is an open source client library for Spotify. It enables applications to use Spotify's service to control and play music via various backends, and to act as a Spotify Connect receiver. It is an alternative to the official and now deprecated closed-source libspotify. Additionally, it will provide extra features which are not available in the official library.
  
  Note: librespot only works with Spotify Premium. This will remain the case. We will not support any features to make librespot compatible with free accounts, such as limited skips and adverts.
endef

define Package/librespot/conffiles
  /etc/config/librespot
endef

define Package/librespot/install
	$(INSTALL_DIR) $(1)/usr/bin
	$(INSTALL_BIN) $(PKG_INSTALL_DIR)/bin/librespot $(1)/usr/bin/librespot
	$(INSTALL_DIR) $(1)/etc/config/
	$(INSTALL_CONF) ./files/librespot.conf $(1)/etc/config/librespot
	$(INSTALL_DIR) $(1)/etc/init.d
	$(INSTALL_BIN) ./files/librespot.init $(1)/etc/init.d/librespot
endef

$(eval $(call RustBinPackage,librespot))
$(eval $(call BuildPackage,librespot))
