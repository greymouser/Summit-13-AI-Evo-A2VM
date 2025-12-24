# Makefile for msi-ctl-led-attach
# Installs script, systemd service, udev rules, and configuration file

PREFIX ?= /usr
DESTDIR ?=

SBINDIR = $(PREFIX)/sbin
SYSTEMD_DIR = $(PREFIX)/lib/systemd/system
UDEV_DIR = $(PREFIX)/lib/udev/rules.d
CONFDIR = /etc/default

INSTALL = install
INSTALL_PROGRAM = $(INSTALL) -m 0755
INSTALL_DATA = $(INSTALL) -m 0644

.PHONY: all install uninstall clean test

all: msi-ctl-led-attach.service

msi-ctl-led-attach.service: msi-ctl-led-attach.service.in
	sed -e 's|@SBINDIR@|$(SBINDIR)|g' $< > $@

install: all
	$(INSTALL) -d $(DESTDIR)$(SBINDIR)
	$(INSTALL) -d $(DESTDIR)$(SYSTEMD_DIR)
	$(INSTALL) -d $(DESTDIR)$(UDEV_DIR)
	$(INSTALL) -d $(DESTDIR)$(CONFDIR)
	
	$(INSTALL_PROGRAM) msi-ctl-led-attach $(DESTDIR)$(SBINDIR)/
	$(INSTALL_DATA) msi-ctl-led-attach.service $(DESTDIR)$(SYSTEMD_DIR)/
	$(INSTALL_DATA) 80-msi-ctl-led.rules $(DESTDIR)$(UDEV_DIR)/
	$(INSTALL_DATA) msi-ctl-led.conf $(DESTDIR)$(CONFDIR)/msi-ctl-led

test:
	systemctl daemon-reload
	systemctl enable msi-ctl-led-attach.service
	udevadm control --reload-rules

uninstall:
	rm -f $(DESTDIR)$(SBINDIR)/msi-ctl-led-attach
	rm -f $(DESTDIR)$(SYSTEMD_DIR)/msi-ctl-led-attach.service
	rm -f $(DESTDIR)$(UDEV_DIR)/80-msi-ctl-led.rules
	rm -f $(DESTDIR)$(CONFDIR)/msi-ctl-led

clean:
	rm -f msi-ctl-led-attach.service
