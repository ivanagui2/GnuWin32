# Makefile for less.

#### Start of system configuration section. ####

srcdir = ../less-394-src
resdir = ./res
VPATH = ../less-394-src $(resdir)

VERSION = 394.0
MAJOR=$(shell echo $(VERSION) | sed -e "s/\..*$$//")
MINOR=$(shell echo $(VERSION) | sed -e "s/^[^\.]*\.0*\([0-9]\+\).*$$/\1/")
VERSADD = -Wl,--major-image-version=$(MAJOR) -Wl,--minor-image-version=$(MINOR) 


CC = gcc
INSTALL = /bin/install.exe -cp
INSTALL_PROGRAM = /bin/install.exe -cp
INSTALL_DATA = /bin/install.exe -cp

CPPFLAGS =   -D__GW32__ -D_LARGEFILE_SOURCE=1 -D_LARGEFILE64_SOURCE=1 -D_FILE_OFFSET_BITS=64 -DWIN32 -D_MSC_VER=1 -DMSDOS_COMPILER=WIN32C     -idirafter D:/Progra~1/GnuWin32/include -idirafter g:/glibc/include    -DINSTALLDIR=\"$(prefix)\" -DENABLE_RELOCATABLE
CFLAGS =  -Wall -O3 -fms-extensions -mms-bitfields -fno-exceptions -fomit-frame-pointer -march=i386 -mcpu=i686 -ffast-math  
CFLAGS_COMPILE_ONLY = -c
LDFLAGS =  -Wl,-s -Wl,--force-exe-suffix -Wl,--enable-auto-import -Wl,--enable-runtime-pseudo-reloc -Wl,--allow-multiple-definition -Wl,--enable-stdcall-fixup -LD:/Progra~1/GnuWin32/lib   	$(VERSADD)
O = o
EXE = .exe

LIBS =  -Wl,-s  -LD:/Progra~1/GnuWin32/lib -lgw32c  -liberty -lintl -liconv -lwsock32 -lole32 -luuid -lmsvcp60   -lcurses -ltermcap -lpcre

prefix = c:/progra~1/Less
exec_prefix = ${prefix}

# Where the installed binary goes.
bindir = ${exec_prefix}/bin
binprefix = 

sysconfdir = ${prefix}/etc

mandir = ${prefix}/man
manext = 1
manprefix = 

#### End of system configuration section. ####

SHELL = /bin/sh

.SUFFIXES:
.SUFFIXES: .c .o .exe .dll.o .rc

# This rule allows us to supply the necessary -D options
# in addition to whatever the user asks for.
.c.o:
	${CC} -I. ${CFLAGS_COMPILE_ONLY} -DBINDIR=\"${bindir}\" -DSYSDIR=\"${sysconfdir}\" ${CPPFLAGS} ${CFLAGS} $<

# Rule to make compiled resource (Windows)
.rc.o:
	windres --include-dir $(resdir) -i $< -o $@

OBJ =	main.${O} screen.${O} brac.${O} ch.${O} charset.${O} cmdbuf.${O} \
	command.${O} decode.${O} edit.${O} filename.${O} forwback.${O} \
	help.${O} ifile.${O} input.${O} jump.${O} line.${O} linenum.${O} \
	lsystem.${O} mark.${O} optfunc.${O} option.${O} opttbl.${O} os.${O} \
	output.${O} position.${O} prompt.${O} search.${O} signal.${O} \
	tags.${O} ttyin.${O} version.${O}   less-res.$(O)

all: less lesskey lessecho

less: ${OBJ}
	${CC} ${LDFLAGS} -o $@ ${OBJ} ${LIBS}

lesskey: lesskey.${O} version.${O} lesskey-res.$(O)
	${CC} ${LDFLAGS} -o $@ lesskey.${O} version.${O} lesskey-res.$(O)

lessecho: lessecho.${O} version.${O} lessecho-res.$(O)
	${CC} ${LDFLAGS} -o $@ lessecho.${O} version.${O} lessecho-res.$(O)

${OBJ}: ${srcdir}/less.h ${srcdir}/funcs.h defines.h 

install: all ${srcdir}/less.nro ${srcdir}/lesskey.nro ${srcdir}/lessecho.nro installdirs
	${INSTALL_PROGRAM} less$(EXE) ${bindir}/${binprefix}less$(EXE)
	${INSTALL_PROGRAM} lesskey$(EXE) ${bindir}/${binprefix}lesskey$(EXE)
	${INSTALL_PROGRAM} lessecho$(EXE) ${bindir}/${binprefix}lessecho$(EXE)
	${INSTALL_DATA} ${srcdir}/less.nro ${mandir}/man${manext}/${manprefix}less.${manext}
	${INSTALL_DATA} ${srcdir}/lesskey.nro ${mandir}/man${manext}/${manprefix}lesskey.${manext}
	${INSTALL_DATA} ${srcdir}/lessecho.nro ${mandir}/man${manext}/${manprefix}lessecho.${manext}

install-strip:
	${MAKE} INSTALL_PROGRAM='${INSTALL_PROGRAM} -s' install

installdirs: mkinstalldirs
	${srcdir}/mkinstalldirs ${bindir} ${mandir}/man${manext}

uninstall:
	rm -f ${bindir}/${binprefix}less ${bindir}/${binprefix}lesskey ${bindir}/${binprefix}lessecho
	rm -f ${mandir}/man${manext}/${manprefix}less.${manext} ${mandir}/man${manext}/${manprefix}lesskey.${manext}

info:
install-info:
dvi:
check:
installcheck:

TAGS:
	cd ${srcdir} && etags *.c *.h

# config.status might not change defines.h
# Don't rerun config.status if we just configured (so there's no stamp-h).
defines.h: stamp-h
stamp-h: defines.h.in config.status
	test ! -f stamp-h || CONFIG_FILES= CONFIG_HEADERS=defines.h ./config.status
	touch stamp-h
Makefile: ${srcdir}/Makefile.in config.status
	CONFIG_FILES=Makefile CONFIG_HEADERS= ./config.status
config.status: ${srcdir}/configure
	./config.status --recheck

${srcdir}/configure: ${srcdir}/configure.ac
	cd ${srcdir}; autoheader; autoconf

clean:
	rm -f *.${O} core less$(EXE) lesskey$(EXE) lessecho$(EXE)

mostlyclean: clean

distclean: clean
	rm -f Makefile config.status config.log config.cache defines.h stamp-h

realclean: distclean
	rm -f TAGS

