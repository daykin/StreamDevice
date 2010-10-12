TOP=../..

# Look if we have EPICS R3.13 or R3.14
ifeq ($(wildcard $(TOP)/configure),)
# EPICS R3.13
include $(TOP)/config/CONFIG_APP
# The real work is in Makefile.Vx
include $(TOP)/config/RULES_ARCHS
else

# EPICS R3.14
include $(TOP)/configure/CONFIG

#HOST_OPT = NO

PROD = streamApp
DBD  = streamApp.dbd

streamApp_DBD += base.dbd
#streamApp_DBD += aaiRecord.dbd aaoRecord.dbd

PROD_SRCS += streamApp_registerRecordDeviceDriver.cpp
PROD_SRCS_DEFAULT = streamAppMain.cc
PROD_SRCS_vxWorks = -nil-

PROD_LIBS = stream

ifdef PCRE
# With local PCRE package
PROD_LIBS += pcre
else
ifneq ($(words $(PCRE_LIB) $(PCRE_INCLUDE)),0)
# With system wide PCRE installation
PROD_SYS_LIBS += pcre
PROD_DEPLIB_DIRS += $(PCRE_LIB)
endif
endif

ifdef ASYN
# Which types of asyn busses do you have?
ifneq ($(OS_CLASS), WIN32)
# asynDriver up to version 4-8 does not support serial port for Windows!
streamApp_DBD += drvAsynSerialPort.dbd
endif
streamApp_DBD += drvAsynIPPort.dbd
#streamApp_DBD += drvGsIP488.dbd
#streamApp_DBD += drvNi1014.dbd
streamApp_DBD += drvVxi11.dbd
PROD_LIBS += asyn
endif

ifneq ($(words $(CALC) $(SYNAPPS)), 0)
# With synApps scalcout record
streamApp_DBD += calc.dbd
streamApp_DBD += streamSynApps.dbd
PROD_LIBS += calc sscan
PROD_LIBS_DEFAULT += streamSynApps
endif

streamApp_DBD += stream.dbd

PROD_LIBS += $(EPICS_BASE_IOC_LIBS)

# Write StreamDevice debug output to this file
CPPFLAGS += -DDEBUGFILE=StreamDebug.log

include $(TOP)/configure/RULES

clean:: myclean

myclean:
	rm -f core* StreamDebug.log

endif

