TOP=../..

include $(TOP)/configure/CONFIG
#----------------------------------------
#  ADD MACRO DEFINITIONS BELOW HERE

# xxxRecord.h will be created from xxxRecord.dbd
DBDINC += xxxRecord

# Install xxxSupport.dbd into <top>/dbd
DBD += xxxSupport.dbd

# Build an IOC support library
LIBRARY_IOC += citestSupport

# Compile and add the code to the support library
citestSupport_SRCS += xxxRecord.c
citestSupport_SRCS += devXxxSoft.c

# Link locally-provided code into the support library,
# rather than directly into the IOC application.
# This is required for Windows DLL builds.
citestSupport_SRCS += dbSubExample.c
citestSupport_SRCS += citestHello.c
citestSupport_SRCS += initTrace.c

citestSupport_LIBS += $(EPICS_BASE_IOC_LIBS)


# Build the IOC application
PROD_IOC = citest

# citest.dbd will be created and installed
DBD += citest.dbd

# citest.dbd will include these files:
citest_DBD += base.dbd
citest_DBD += xxxSupport.dbd
citest_DBD += dbSubExample.dbd
citest_DBD += citestHello.dbd
citest_DBD += initTrace.dbd

# citest_registerRecordDeviceDriver.cpp derives from citest.dbd
citest_SRCS += citest_registerRecordDeviceDriver.cpp

# Build the main IOC entry point where needed
citest_SRCS_DEFAULT += citestMain.cpp
citest_SRCS_vxWorks += -nil-

# Link in the code from our support library
citest_LIBS += citestSupport

# To build SNL programs, SNCSEQ must be defined
# in the <top>/configure/RELEASE file
ifneq ($(SNCSEQ),)
    # Build sncExample into citestSupport
    sncExample_SNCFLAGS += +r
    citest_DBD += sncExample.dbd
    # A .stt sequence program is *not* pre-processed:
    citestSupport_SRCS += sncExample.stt
    citestSupport_LIBS += seq pv
    citest_LIBS += seq pv

    # Build sncProgram as a standalone program
    PROD_HOST += sncProgram
    sncProgram_SNCFLAGS += +m
    # A .st sequence program *is* pre-processed:
    sncProgram_SRCS += sncProgram.st
    sncProgram_LIBS += seq pv
    sncProgram_LIBS += $(EPICS_BASE_HOST_LIBS)
endif

# Finally link IOC to the EPICS Base libraries
citest_LIBS += $(EPICS_BASE_IOC_LIBS)

include $(TOP)/configure/RULES
#----------------------------------------
#  ADD EXTRA GNUMAKE RULES BELOW HERE

