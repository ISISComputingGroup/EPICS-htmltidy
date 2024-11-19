TOP = ../..
include $(TOP)/configure/CONFIG

## setting MAKEFLAGS causes cmake issues
override MAKEFLAGS=

CMAKE ?= cmake

# extract visual studio major version number from path
# unfortunately cannot use %.% so using %.0 but may not match minor version number changes e.g. 10.1
ifneq ($(VisualStudioVersion),)
VS_VERS=$(filter %.0,$(subst \, ,$(VisualStudioVersion)))
else
VS_VERS=$(filter %.0,$(subst \, ,$(VSINSTALLDIR)))
endif
VS_MAJOR=$(word 1,$(subst ., ,$(VS_VERS)))

INSTALL_DIR=$(abspath $(TOP)/install/$(EPICS_HOST_ARCH))
CMAKE_CONFIG_FLAGS=-DBUILD_TAB2SPACE:BOOL=ON 
CMAKE_BUILD_FLAGS=
ifneq ($(findstring linux,$(EPICS_HOST_ARCH)),)
CMAKE_GENERATOR=Unix Makefiles
else
ifneq ($(findstring windows,$(EPICS_HOST_ARCH)),)
VS_ARCH=$(VS_MAJOR)
PLATFORM=x64
else
VS_ARCH=$(VS_MAJOR)
PLATFORM=Win32
endif
CMAKE_GENERATOR=Visual Studio $(VS_ARCH)
endif
ifneq ($(findstring debug,$(EPICS_HOST_ARCH)),)
CMAKE_CONFIG_FLAGS += -DCMAKE_BUILD_TYPE:STRING=Debug
CMAKE_CONFIG=Debug
else
CMAKE_CONFIG_FLAGS += -DCMAKE_BUILD_TYPE:STRING=RelWithDebInfo
CMAKE_CONFIG=RelWithDebInfo
endif
ifneq ($(findstring static,$(EPICS_HOST_ARCH)),)
CMAKE_CONFIG_FLAGS += -DBUILD_SHARED_LIB:BOOL=OFF -DUSE_STATIC_RUNTIME:BOOL=ON
endif

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD RULES AFTER THIS LINE

# we need to define DISABLE_DEBUG_LOG or otherwise on a Debug buidl we get lots of extra details

ifdef T_A
install:
	$(CMAKE) -E env CFLAGS=-DDISABLE_DEBUG_LOG $(CMAKE) $(TOP)/htmltidy -G "$(CMAKE_GENERATOR)" -A "$(PLATFORM)" -DCMAKE_INSTALL_PREFIX:PATH="$(INSTALL_DIR)" $(CMAKE_CONFIG_FLAGS)
	$(CMAKE) --build . --target install --config $(CMAKE_CONFIG) $(CMAKE_BUILD_FLAGS)
	-$(MKDIR) $(TOP)/bin
	-$(MKDIR) $(TOP)/bin/$(EPICS_HOST_ARCH)
	-$(MKDIR) $(TOP)/lib
	-$(MKDIR) $(TOP)/lib/$(EPICS_HOST_ARCH)
	-$(MKDIR) $(TOP)/include
	$(CP) -f $(INSTALL_DIR)/include/*.h* $(TOP)/include/
ifeq ($(findstring linux,$(EPICS_HOST_ARCH)),)
ifneq ($(findstring static,$(EPICS_HOST_ARCH)),)
	$(CP) -f $(INSTALL_DIR)/lib/tidy_static*.lib $(TOP)/lib/$(EPICS_HOST_ARCH)/tidy.lib
else
	$(RM) -f $(INSTALL_DIR)/lib/tidy_static*.lib
	$(CP) -f $(INSTALL_DIR)/lib/tidy*.lib $(TOP)/lib/$(EPICS_HOST_ARCH)/tidy.lib
endif
else
	$(CP) -f $(INSTALL_DIR)/lib/libtidy.* $(TOP)/lib/$(EPICS_HOST_ARCH)/
endif
	$(CP) -f $(INSTALL_DIR)/bin/* $(TOP)/bin/$(EPICS_HOST_ARCH)/
endif
