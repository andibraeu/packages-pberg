include $(TOPDIR)/rules.mk

PKG_NAME:=luci-weimar-splash
PKG_RELEASE:=0.1.0

PKG_BUILD_DIR := $(BUILD_DIR)/$(PKG_NAME)

include $(INCLUDE_DIR)/package.mk

define Package/luci-weimar-splash
  SECTION:=luci
  CATEGORY:=LuCI
  SUBMENU:=3. Applications
  TITLE:=Weimarnetz Splashpage
endef

define Package/luci-weimar-splash/description
  provide a weimar/kalua specific page
  that allows to load current information
endef

define Build/Prepare
	mkdir -p $(PKG_BUILD_DIR)
	$(CP) ./luasrc $(PKG_BUILD_DIR)/
endef

define Build/Configure
endef

define Build/Compile
	$(MAKE) -C $(PKG_BUILD_DIR)/luasrc
endef

define Package/luci-weimar-splash/postinst
#!/bin/sh
[ -n "${IPKG_INSTROOT}" ] || {
	uci set uhttpd.main.index_page="weimar-splash-index.html"
	uci set uhttpd.main.error_page="/weimar-splash-index.html"
	uci commit
	rm -f /tmp/luci-indexcache
	exit 0
}
endef

define Package/luci-weimar-splash/install
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/controller
	$(INSTALL_DIR) $(1)/usr/lib/lua/luci/view
	$(CP) $(PKG_BUILD_DIR)/luasrc/controller/* $(1)/usr/lib/lua/luci/controller
	$(CP) $(PKG_BUILD_DIR)/luasrc/view/* $(1)/usr/lib/lua/luci/view
	$(INSTALL_DIR) $(1)/www/luci-static/resources
	$(CP) ./luci-static/resources/* $(1)/www/luci-static/resources
	$(CP) ./root/* $(1)/www/
endef

$(eval $(call BuildPackage,luci-weimar-splash))