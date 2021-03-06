#
# Copyright (C) 2013 Arduino LLC. All right reserved
#
# This is free software, licensed under the GNU General Public License v2.
# See /LICENSE for more information.
#

include $(TOPDIR)/rules.mk

PKG_NAME:=ideino
PKG_VERSION:=0.0.5
PKG_RELEASE:=2

#PKG_SOURCE:=v$(PKG_VERSION).tar.gz
#PKG_SOURCE_URL:=https://github.com/ideino/ideino-linino-dist/archive/v${PKG_VERSION}
GIT_SOURCE:=https://github.com/ideino/ideino-linino-dist.git

include $(INCLUDE_DIR)/package.mk

define Package/ideino
  SECTION:=utils
  CATEGORY:=Development
  DEFAULT:=m
  TITLE:=A simple but effective IDE for Node.Js
  DEPENDS:=+libpthread +libstdcpp +node
  # DESCRIPTION:=This variable is obsolete. use the Package/name/description define instead!
  URL:=https://github.com/ideino/ideino-linino-dist
endef

define Package/ideino/description
  Node.Js IDE
endef

define Build/Compile
 # NOOP
endef
#git clone ${GIT_SOURCE} -b v${PKG_VERSION} ideino-linino)

define Build/Prepare
	(cd $(PKG_BUILD_DIR);\
	git clone ${GIT_SOURCE} -b master ideino-linino;\
	cd ideino-linino;\
	rm -rf .git;\
	rm .gitignore);
endef

define Package/ideino/install
  	$(INSTALL_DIR) $(1)

	$(CP) $(PKG_BUILD_DIR)/ideino-linino $(1)
endef

define Package/ideino/postinst
#!/bin/sh
# # check if we are on real system
if [ -d /opt/ideino-linino ] && [ -e /opt/ideino-linino/_lininofiles/ideino ] && [ -e /opt/ideino-linino/_lininofiles/ideinoautorun ] && [ -e /opt/ideino-linino/_lininofiles/ideino.js ] && [ -e /opt/ideino-linino/_lininofiles/homepage.htm ]
then
	cp /opt/ideino-linino/_lininofiles/ideino.js /www/luci-static/resources/linino
	mv /usr/lib/lua/luci/view/linino/homepage.htm /usr/lib/lua/luci/view/linino/homepage.htm.bak
	cp /opt/ideino-linino/_lininofiles/homepage.htm /usr/lib/lua/luci/view/linino/
	#
	cp /opt/ideino-linino/_lininofiles/ideino /etc/init.d
	cp /opt/ideino-linino/_lininofiles/ideinoautorun /etc/init.d
	#
	chmod +x /etc/init.d/ideino
	chmod +x /etc/init.d/ideinoautorun
	#
	echo "Enabling rc.d symlink for ideino"
 	/etc/init.d/ideino enable
 	/etc/init.d/ideinoautorun enable
fi
exit 0
endef

define Package/ideino/prerm
#!/bin/sh
# # check if we are on real system
if [ -x /etc/init.d/ideino ] && [ -x /etc/init.d/ideinoautorun ]
then
	ps www | grep -i [n]ode | awk {'print $1'} | xargs kill -9
	echo "Disabling rc.d symlink for ideino"
	/etc/init.d/ideino disable
 	/etc/init.d/ideinoautorun disable
fi
exit 0
endef

define Package/ideino/postrm
#!/bin/sh
# # check if we are on real system
if [ -d /opt/ideino-linino ] && [ -e /etc/init.d/ideino ] && [ -e /etc/init.d/ideinoautorun ] && [ -e /www/luci-static/resources/linino/ideino.js ] && [ -e /usr/lib/lua/luci/view/linino/homepage.htm ] && [ -e /usr/lib/lua/luci/view/linino/homepage.htm.bak ]
then
	rm -rf /opt/ideino-linino
	rm /etc/init.d/ideino && rm /etc/init.d/ideinoautorun
	rm /www/luci-static/resources/linino/ideino.js
	rm /usr/lib/lua/luci/view/linino/homepage.htm
	mv /usr/lib/lua/luci/view/linino/homepage.htm.bak /usr/lib/lua/luci/view/linino/homepage.htm	
fi
exit 0
endef

$(eval $(call BuildPackage,ideino))
