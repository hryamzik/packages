include $(TOPDIR)/rules.mk

PKG_NAME:=utr-lcd
PKG_VERSION:=0.1.0
PKG_RELEASE:=1

PKG_SOURCE:=$(PKG_NAME)-$(PKG_VERSION).tar.gz
PKG_SOURCE_URL:=https://codeload.github.com/hryamzik/unifi/tar.gz/v$(PKG_VERSION)?
PKG_HASH:=e8b8dacd638d943416fc09aa2891b716f5c5bf8d364268bef6deaa6685869c7f
# the upstream repository (and its tarball) is called "unifi", the package is "utr-lcd"
PKG_BUILD_DIR:=$(BUILD_DIR)/unifi-$(PKG_VERSION)

PKG_MAINTAINER:=Roman Belyakovsky <roman.beliakovskii@smartly.io>
PKG_LICENSE:=MIT
PKG_LICENSE_FILES:=package/utr-lcd/LICENSE

include $(INCLUDE_DIR)/package.mk

define Package/utr-lcd
  SECTION:=utils
  CATEGORY:=Utilities
  TITLE:=Status display for the UniFi Travel Router (UTR/UTR-LR) LCD
  # the LCD panel driver (kmod-drm-panel-mipi-dbi) comes with the device image; the init script
  # checks for /dev/dri/card0 at start
  DEPENDS:=@TARGET_ipq40xx +ubus +ubusd
endef

define Package/utr-lcd/description
  Shows Wi-Fi/Ethernet uplink, WAN IP, VPN, connected clients and uptime on the built-in
  136x240 LCD of the UniFi Travel Router. Works out of the box with a built-in renderer. If
  the user installs the optional stock bundle (made from their own Ubiquiti firmware) the
  original Ubiquiti-style UI is shown instead.
endef

define Package/utr-lcd/conffiles
/etc/config/utr-lcd
endef

UTR_SRC:=$(PKG_BUILD_DIR)/package/utr-lcd

define Build/Compile
	$(MAKE) -C $(UTR_SRC)/src \
		CC="$(TARGET_CC)" \
		CFLAGS="$(TARGET_CFLAGS)" \
		LDFLAGS="$(TARGET_LDFLAGS)"
endef

define Package/utr-lcd/install
	$(INSTALL_DIR) $(1)/usr/bin $(1)/usr/sbin $(1)/usr/lib/utr-lcd $(1)/usr/libexec/utr-lcd $(1)/etc/init.d $(1)/etc/config
	$(INSTALL_BIN) $(UTR_SRC)/src/utr-lcd-status $(1)/usr/bin/
	$(INSTALL_BIN) $(UTR_SRC)/src/utr-lcd-fb2drm $(1)/usr/bin/
	$(INSTALL_BIN) $(UTR_SRC)/src/utr-lcd-fbshim.so $(1)/usr/lib/utr-lcd/
	$(INSTALL_BIN) $(UTR_SRC)/files/collect.sh $(1)/usr/libexec/utr-lcd/collect.sh
	$(INSTALL_BIN) $(UTR_SRC)/files/feed.sh $(1)/usr/libexec/utr-lcd/feed.sh
	$(INSTALL_BIN) $(UTR_SRC)/files/traffic.sh $(1)/usr/libexec/utr-lcd/traffic.sh
	$(INSTALL_BIN) $(UTR_SRC)/files/power.sh $(1)/usr/libexec/utr-lcd/power.sh
	$(INSTALL_BIN) $(UTR_SRC)/files/ui-call.sh $(1)/usr/libexec/utr-lcd/ui-call.sh
	$(INSTALL_BIN) $(UTR_SRC)/files/stock-run.sh $(1)/usr/libexec/utr-lcd/stock-run.sh
	$(INSTALL_BIN) $(UTR_SRC)/files/stock-install.sh $(1)/usr/sbin/utr-lcd-stock-install
	$(INSTALL_BIN) $(UTR_SRC)/files/utr-lcd.init $(1)/etc/init.d/utr-lcd
	$(INSTALL_CONF) $(UTR_SRC)/files/utr-lcd.config $(1)/etc/config/utr-lcd
endef

$(eval $(call BuildPackage,utr-lcd))
