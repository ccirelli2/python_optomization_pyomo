# Copyright (C) 2003, 2006 International Business Machines and others.
# All Rights Reserved.
# This file is distributed under the Common Public License.

## $Id: Makefile.am 759 2006-07-07 03:07:08Z andreasw $

AUTOMAKE_OPTIONS = foreign

EXTRA_DIST = \
	CUTErInterface.f \
	ipo.pro \
	ipoma-dummy.f \
	sdipo.pro

noinst_LTLIBRARIES = libdummy.la

nodist_libdummy_la_SOURCES = ipoma.f

# This line is necessary to allow VPATH compilation with MS compilers
# on Cygwin
DEFAULT_INCLUDES = -I. -I`$(CYGPATH_W) $(srcdir)` -I$(top_builddir)/inc

ipoma.f: CUTErInterface.f
	if test -z "$$MYCUTER" -o -z "$$CUTER"; then \
	  rm -f  ipoma.f; \
	  cp $(srcdir)/ipoma-dummy.f ipoma.f; \
	else \
	  sed "/#{sizing}/r $$MYCUTER/double/config/tools.siz" $< | sed -f $$MYCUTER/double/config/cast.sed | sed "s/C$(BITS_PER_POINTER)BIT//" > ipoma.f; \
	fi

ipo: ipo.pro
	sed -f $$MYCUTER/double/config/script.sed $< > $@
	chmod a+x $@

sdipo: sdipo.pro
	sed -f $$MYCUTER/double/config/script.sed $< > $@
	chmod a+x $@

IPADDLIBS = @IPADDLIBS@

CLEANFILES = ipoma.f ipo sdipo

cuter:	ipoma.$(OBJEXT) ipo sdipo
	if test -z "$$MYCUTER" -o -z "$$CUTER"; then \
	  echo CUTEr environment variables not set - cannot install ipoma.f; \
	else \
	  echo "-L$(libdir) -lipopt $(ADDLIBS) $(CXXLIBS)" >$$MYCUTER/double/lib/ipo.liblist ;\
	  cp ipoma.o $(MYCUTER)/double/bin/ipoma.$(OBJEXT); \
	  cp ipo $(MYCUTER)/bin/ipo ;\
	  cp sdipo $(MYCUTER)/bin/sdipo ;\
	fi
