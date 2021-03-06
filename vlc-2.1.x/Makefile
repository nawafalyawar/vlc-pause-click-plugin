PREFIX = /usr
LD = ld
CC = cc
INSTALL = install
CFLAGS = -g -O2 -Wall -Wextra
LDFLAGS =
VLC_PLUGIN_CFLAGS := $(shell pkg-config --cflags vlc-plugin)
VLC_PLUGIN_LIBS := $(shell pkg-config --libs vlc-plugin)

libdir = $(PREFIX)/lib

# Slackware and openSUSE use /usr/lib64 on x86_64
ARCH=$(shell uname -m)
ifeq ($(ARCH), x86_64)
    lib64Test=$(shell grep -E "Slackware|openSUSE" /etc/*-release | wc -l)
    ifneq ($(lib64Test), 0)
        libdir = $(PREFIX)/lib64
    endif
endif

plugindir = $(libdir)/vlc/plugins

override CC += -std=gnu99
override CPPFLAGS += -DPIC -I. -Isrc
override CFLAGS += -fPIC
override LDFLAGS += -Wl,-no-undefined,-z,defs

override CPPFLAGS += -DMODULE_STRING=\"pause_click\"
override CFLAGS += $(VLC_PLUGIN_CFLAGS)
override LDFLAGS += $(VLC_PLUGIN_LIBS)

TARGETS = libpause_click_plugin.so

all: libpause_click_plugin.so

install: all
	mkdir -p -- $(DESTDIR)$(plugindir)/video_filter
	$(INSTALL) --mode 0755 libpause_click_plugin.so $(DESTDIR)$(plugindir)/video_filter

install-strip:
	$(MAKE) install INSTALL="$(INSTALL) -s"

uninstall:
	rm -f $(plugindir)/video_filter/libpause_click_plugin.so

clean:
	rm -f -- libpause_click_plugin.so pause_click.o

mostlyclean: clean

SOURCES = pause_click.c

$(SOURCES:%.c=%.o): %: pause_click.c

libpause_click_plugin.so: $(SOURCES:%.c=%.o)
	$(CC) -shared -o $@ $^ $(LDFLAGS)

.PHONY: all install install-strip uninstall clean mostlyclean
