TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS AFTER THIS LINE
#=============================

#==================================================
# build a support library

LIBRARY_IOC += htmltidy
PROD_IOC += tidy tab2space

TIDYDIR = $(TOP)/htmltidy
INCDIR = $(TIDYDIR)/include

htmltidy_CFLAGS += -I$(INCDIR) 

USR_CFLAGS_WIN32 += -DWIN32 -DSUPPORT_UTF16_ENCODINGS=1 -DSUPPORT_ASIAN_ENCODINGS=1 -DSUPPORT_ACCESSIBILITY_CHECKS=1

ifeq ($(SHARED_LIBRARIES),YES)
htmltidy_CFLAGS_WIN32 += -DTIDY_EXPORT
htmltidy_LDFLAGS_WIN32 += /DEF:$(TIDYDIR)/build/msvc/tidy.def
tidy_CFLAGS_WIN32 += -DTIDY_EXPORT=__declspec(dllimport)
endif

ifeq ($(STATIC_BUILD),YES)
htmltidy_CFLAGS_WIN32 += -DTIDY_EXPORT=extern
tidy_CFLAGS_WIN32 += -DTIDY_EXPORT=extern
endif

# _LIB and _USRDLL ?-UWINDOWS -D_MBCS -D_DEBUG=1

#XSLTPROC = xsltproc

#DOCS = $(DOCDIR)/quickref.html $(DOCDIR)/tidy.1

#CONFIGXML = $(DOCDIR)/tidy-config.xml
#HELPXML = $(DOCDIR)/tidy-help.xml

SRC_DIRS += $(TIDYDIR)/src $(TIDYDIR)/console $(TIDYDIR)/include

htmltidy_SRCS += \
        access.c       attrs.c        istack.c \
        parser.c       tags.c         entities.c \
        lexer.c        pprint.c       clean.c \
        localize.c     config.c       alloc.c \
        attrask.c      attrdict.c     attrget.c \
        buffio.c       fileio.c       streamio.c \
        tagask.c       tmbstr.c       utf8.c \
        tidylib.c      mappedio.c

htmltidy_SRCS_WIN32 += win32tc.c

TIDY_INCS = platform.h  tidy.h  tidyenum.h buffio.h
#INC += $(TIDY_INCS)

tidy_SRCS += tidy.c
tab2space_SRCS += tab2space.c

#$(HELPXML): $(BINDIR)/$(PROJECT)
#	$(BINDIR)/$(PROJECT) -xml-help > $@

#$(CONFIGXML): $(BINDIR)/$(PROJECT)
#	$(BINDIR)/$(PROJECT) -xml-config > $@

#$(DOCDIR)/quickref.html: $(DOCDIR)/quickref-html.xsl $(CONFIGXML)
#	$(XSLTPROC) -o $@ $(DOCDIR)/quickref-html.xsl $(CONFIGXML)

#$(DOCDIR)/tidy.1: $(DOCDIR)/tidy1.xsl $(HELPXML) $(CONFIGXML)
#	$(XSLTPROC) -o $@ $(DOCDIR)/tidy1.xsl $(HELPXML)

htmltidy_LIBS += $(EPICS_BASE_HOST_LIBS)

tidy_LIBS += htmltidy
tidy_LIBS += $(EPICS_BASE_HOST_LIBS)

tab2space_LIBS += $(EPICS_BASE_HOST_LIBS)

#===========================

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD RULES AFTER THIS LINE
install:
	$(INSTALL) -d -m $(INSTALL_PERMISSIONS) $(foreach f,$(TIDY_INCS),$(TIDYDIR)/include/$(f)) $(INSTALL_INCLUDE)

