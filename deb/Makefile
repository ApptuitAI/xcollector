# This file is part of xcollector.
# Copyright (C) 2017  AgilX Inc.
#

PACKAGE_VERSION := 0.6
PACKAGE_REVISION := 1

# Various commands we need.
# In some environment this command can be called `gsha1sum' or `digest'.
PYTHON_VERSION := 2.6
PYTHON := python$(PYTHON_VERSION)

# Our "source tree" is in the parent directory, always.
VPATH := ..

git_version := \
  `git rev-list --pretty=format:%h HEAD --max-count=1 | sed 1d || echo unknown`
git_fullsha1 := \
  `git rev-list --pretty=format:%H HEAD --max-count=1 | sed 1d || echo unknown`

# What / where do we build.
DEB_TARGET := amd64
BASENAME := xcollector_$(PACKAGE_VERSION)-$(PACKAGE_REVISION)
DEB := $(BASENAME)_$(DEB_TARGET).deb

all: clean deb

deb:
	mkdir -p dist/
	mkdir -p build/DEBIAN/
	mkdir -p build/usr/local/xcollector/collectors/0/
	mkdir -p build/usr/local/xcollector/collectors/etc/
	mkdir -p build/usr/local/xcollector/collectors/lib/
	mkdir -p build/usr/local/xcollector/conf/
	cp -r DEBIAN/* build/DEBIAN/
	cp -r etc/ build/
	cp $(VPATH)/collectors/0/dfstat.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/0/grok_exporter.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/0/ifstat.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/0/iostat.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/0/memcache.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/0/mysql.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/0/netstat.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/0/procnettcp.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/0/procstats.py build/usr/local/xcollector/collectors/0/
	cp $(VPATH)/collectors/__init__.py build/usr/local/xcollector/collectors/
	cp $(VPATH)/collectors/etc/__init__.py build/usr/local/xcollector/collectors/etc/
	cp $(VPATH)/collectors/etc/config.py build/usr/local/xcollector/collectors/etc/
	cp $(VPATH)/collectors/etc/grok_exporter_conf.py build/usr/local/xcollector/collectors/etc/
	cp $(VPATH)/collectors/etc/metric_naming.py build/usr/local/xcollector/collectors/etc/
	cp $(VPATH)/collectors/etc/mysqlconf.py build/usr/local/xcollector/collectors/etc/
	cp $(VPATH)/collectors/etc/yaml_conf.py build/usr/local/xcollector/collectors/etc/
	cp $(VPATH)/collectors/lib/__init__.py build/usr/local/xcollector/collectors/lib/
	cp $(VPATH)/collectors/lib/utils.py build/usr/local/xcollector/collectors/lib/
	cp $(VPATH)/conf/*.yml build/usr/local/xcollector/conf/
	cp -r $(VPATH)/grok_exporter-0.2.1.linux-amd64/ build/usr/local/xcollector/
	mv build/usr/local/xcollector/grok_exporter-0.2.1.linux-amd64/grok_exporter_deb build/usr/local/xcollector/grok_exporter-0.2.1.linux-amd64/grok_exporter
	rm build/usr/local/xcollector/grok_exporter-0.2.1.linux-amd64/grok_exporter_rpm
	cp $(VPATH)/tcollector.py build/usr/local/xcollector/xcollector.py
	set -e; { \
	  sed \
            -e 's/@PACKAGE_VERSION@/$(PACKAGE_VERSION)/' \
            -e 's/@PYTHON_VERSION@/$(PYTHON_VERSION)/' \
            -e "s/@GIT_SHORTSHA1@/$(git_version)/" \
            -e "s/@GIT_FULLSHA1@/$(git_fullsha1)/" \
            -e 's/@PACKAGE_REVISION@/$(PACKAGE_REVISION)/' \
	    DEBIAN/control; \
        } >build/DEBIAN/control
	dpkg-deb -b build dist/$(DEB)

clean:
	rm -rf build dist

.PHONY: all deb clean
.SUFFIXES: .deb