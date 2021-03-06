#============================================================================
# Name        : Makefile (transtreamproxy)
# Author      : oskwon(oskwon@dev3)
# Version     : 
# Copyright   : Copyright(c)2014 Vu+ Team. All right reserved.
# Description :
#============================================================================

-include config.mk

ifeq ($(MODEL),)
$(error config.mk is not set. please run script.config before make.)
endif

MAJOR = 3
MINOR = 0
PROJECT = transtreamproxy

TOP=$(PWD)/..
OBJ=./obj/

ifeq ($(CROSS),YES)
SYSROOT=$(OETOP)/build/$(MODEL)/tmp/sysroots/$(MODEL)
TOOLCHAIN=$(OETOP)/build/$(MODEL)/tmp/sysroots/i686-linux/usr/bin/mipsel-oe-linux/mipsel-oe-linux-
endif

RM=rm -Rf
CXX=$(TOOLCHAIN)g++ --sysroot=$(OETOP)/build/$(MODEL)/tmp/sysroots/$(MODEL)
LD=$(TOOLCHAIN)ld --sysroot=$(OETOP)/build/$(MODEL)/tmp/sysroots/$(MODEL)
STRIP=$(TOOLCHAIN)strip
UPLOAD=./script.upload

ifeq ($(MODE),DEBUG)
CFLAGS += -g
else
CFLAGS += -O2
endif
CFLAGS += -D_MAJOR=$(MAJOR) -D_MINOR=$(MINOR) -D_GNU_SOURCE -D_FILE_OFFSET_BITS=64
CFLAGS += -I$(SYSROOT)/usr/include -I$(TOP)/build
LDFLAGS += -L$(SYSROOT)/usr/lib -lpthread -lrt

SRCS = $(shell find ../src/ -name "*.cpp")
OBJS = $(SRCS:.cpp=.o)
CFLAGS += $(addprefix -I, $(shell find ../src/ -type d))

.SUFFIXES : .cpp .o
.PHONY : all clean install .showinfo .prepare $(PROJECT)

.cpp.o: 
	$(CXX) -c $(CFLAGS) -o $(OBJ)$(notdir $@) $<

all: .showinfo .prepare $(PROJECT)

$(PROJECT):$(OBJS)
	$(CXX) -o $@ $(addprefix $(OBJ), $(notdir $(OBJS))) $(LDFLAGS)
	$(STRIP) $@

install:
	$(UPLOAD) $(UPIP) . $(PROJECT) $(UPDIR)

clean:
	$(RM) $(PROJECT) obj *.o *.a *.d *.log

.prepare:
	echo "" > config.h
	@if [ ! -e obj ]; then mkdir obj; fi

.showinfo:
	@echo "-----------------------------------------------------"
	@echo "                [ BUILD ENVIRONMENT ]                "
	@echo "-----------------------------------------------------"
	@echo "PROJECT  : "$(PROJECT)" (v"$(MAJOR)"."$(MINOR)")"
	@echo ""
	@echo "CXX      : "$(CXX)
	@echo "LD       : "$(LD)
	@echo "STRIP    : "$(STRIP)
	@echo "CFLAGS   : "$(CFLAGS)
	@echo "LDFLAGS  : "$(LDFLAGS)
	@echo "-----------------------------------------------------"
	@echo
