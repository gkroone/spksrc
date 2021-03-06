SPK_NAME = ffsync
SPK_VERS = 1.5.2
SPK_REV = 2
SPK_ICON = src/ffsync.png
BETA = 1

UNSUPPORTED_ARCHS = 88f5281

BUILD_DEPENDS = cross/python cross/setuptools cross/pip cross/wheel
BUILD_DEPENDS += cross/gevent cross/greenlet cross/simplejson
BUILD_DEPENDS += cross/umemcache

DEPENDS     = cross/busybox
WHEELS      = src/requirements.txt
SPK_DEPENDS = "python>2.7.6-8"

MAINTAINER   = SynoCommunity
DESCRIPTION  = Firefox Sync Server 1.5, used for Firefox 29 and later. You can use Firefox Sync Server to synchronize your bookmarks, passwords, settings, history, add-ons and tabs with Firefox on other computers. The service runs on port 8132.
DISPLAY_NAME = Firefox Sync Server 1.5
CHANGELOG = "1. Update to 1.5.2<br>2. Add MariaDB dependency"

HOMEPAGE     = https://docs.services.mozilla.com/howtos/run-sync-1.5.html
LICENSE      = MPL/GPL/LGPL
HELPURL      = https://github.com/SynoCommunity/spksrc/wiki/Firefox-Sync-Server-1.5

WIZARDS_DIR      = src/wizard/
CONF_DIR         = src/conf/
INSTALLER_SCRIPT = src/installer.sh
SSS_SCRIPT       = src/dsm-control.sh
FWPORTS          = src/${SPK_NAME}.sc

INSTALL_DEP_SERVICES = mysql
START_DEP_SERVICES = mysql

INSTALL_PREFIX = /usr/local/$(SPK_NAME)

POST_STRIP_TARGET = ffsync_extra_install

BUSYBOX_CONFIG  = usermng
ENV += BUSYBOX_CONFIG="$(BUSYBOX_CONFIG)"

include ../../mk/spksrc.spk.mk

.PHONY: ffsync_extra_install
ffsync_extra_install:
	install -m 755 -d $(STAGING_DIR)/app
	install -m 755 -d $(STAGING_DIR)/var
	install -m 644 src/ffsync.ini $(STAGING_DIR)/var/
	install -m 755 -d $(STAGING_DIR)/share
	install -m 755 -d $(STAGING_DIR)/share/wheelhouse	
	install -m 644 $(WORK_DIR)/wheelhouse/* $(STAGING_DIR)/share/wheelhouse/
	install -m 755 -d $(STAGING_DIR)/app/images
	for size in 16 24 32 48; do \
		convert $(SPK_ICON) -thumbnail $${size}x$${size} \
			$(STAGING_DIR)/app/images/$(SPK_NAME)-$${size}.png ; \
	done
	sed -i -e "s|https://github.com/mozilla-services/mozservices/archive/e00e1b68130423ad98d0f6185655bde650443da8.zip|mozsvc==0.8|g" \
	       -e "s|https://github.com/mozilla-services/tokenserver/archive/d7e513e8a4f5c588b70d685a8df1d2e508c341c0.zip|tokenserver==1.2.7|g" \
	       -e "s|http://github.com/mozilla-services/server-syncstorage/archive/1.5.5.zip|SyncStorage==1.5.5|g" \
	       -e "s|https://github.com/mozilla-services/syncserver/archive/1.5.2.tar.gz|syncserver==1.5.2|g" \
                $(STAGING_DIR)/share/wheelhouse/requirements.txt
