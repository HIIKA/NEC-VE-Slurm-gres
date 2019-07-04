# Makefile for gres/ve plugin

AUTOMAKE_OPTIONS = foreign

PLUGIN_FLAGS = -module -avoid-version --export-dynamic

AM_CPPFLAGS = -I$(top_srcdir) -I$(top_srcdir)/src/common

pkglib_LTLIBRARIES = gres_ve.la

# Gres VE plugin.
gres_ve_la_SOURCES = gres_ve.c
gres_ve_la_LDFLAGS = $(PLUGIN_FLAGS)
gres_ve_la_LIBADD = ../common/libgres_common.la

force:
$(gres_mic_la_LIBADD) : force
	@cd `dirname $@` && $(MAKE) `basename $@`
