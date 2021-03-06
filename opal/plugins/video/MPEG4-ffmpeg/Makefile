#
# Makefile for MPEG4 video plugin for Unix
#
# Copyright (C) 2007 Post Increment, All Rights Reserved
#
# The contents of this file are subject to the Mozilla Public License
# Version 1.0 (the "License"); you may not use this file except in
# compliance with the License. You may obtain a copy of the License at
# http://www.mozilla.org/MPL/
#
# Software distributed under the License is distributed on an "AS IS"
# basis, WITHOUT WARRANTY OF ANY KIND, either express or implied. See
# the License for the specific language governing rights and limitations
# under the License.
#
# The Original Code is Open H323 library.
#
# The Initial Developer of the Original Code is Post Increment
#
# Contributor(s): ______________________________________.
#
# $Log: Makefile.in,v $
# Revision 1.4  2007/09/02 12:00:50  dominance
# * Improved x264 and libavcodec detection: version check & library name check
# * changed BOOL to bool in mpeg4 ( completion of previous commit)
#
# patch received from and thus thanks goes to Matthias Schneider.
#
# Revision 1.3  2007/08/29 19:32:20  dsandras
# Applied cleanup patch from Matthias Schneider <ma30002000 yahoo de> :
# * make use of common plugin code
# * added debug traces
# * switch to new plugin capability API
# Thanks !
#
# Revision 1.2  2007/06/02 12:28:13  dsandras
# Fixed various aspects of the build system for the MPEG-4 plugin thanks
# to Michael Smith <msmith cbnco com>. Thanks a lot!
#
# Revision 1.1  2007/04/19 07:40:31  csoutheren
# Applied 1703212 - MPEG4 plugin
# Thanks to Josh Mahonin and Michael Smith of Canadian Bank Note, Limited
# and Guilheim Tardy of Salyens
#
#

VC_PLUGIN_DIR=opal-3.9.0/codecs/video
prefix=/opt/t38modem
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
target_os=linux-gnu
DL_LIBS =-ldl

BASENAME= mpeg4-ffmpeg
SONAME	=$(BASENAME)
COMMONDIR=../common
PLUGINDIR=../../

CC	 	=gcc
CFLAGS	 	= -Os 
CXX	 	=g++
LDSO	 	=-shared -Wl,-soname,$(SONAME)
PLUGINEXT	=so
STDCCFLAGS	= -fPIC
LDFLAGS		=

vpath   %.cxx $(COMMONDIR)
vpath   %.o   $(OBJDIR)

SRCS	+= mpeg4.cxx \
	   $(COMMONDIR)/dyna.cxx \
	   $(COMMONDIR)/trace.cxx

LIBAVCODEC_CFLAGS     = 
LIBAVCODEC_SOURCE_DIR = 

# Add LIBAVCODEC_SOURCE_DIR to the include path so we can #include <libavcodec/...h>
# Also add libavutil, so ffmpeg headers can #include "log.h".
EXTRACCFLAGS    += $(LIBAVCODEC_CFLAGS) -I$(COMMONDIR) -I$(PLUGINDIR)

ifneq (,$(LIBAVCODEC_SOURCE_DIR))
EXTRACCFLAGS    += -I$(LIBAVCODEC_SOURCE_DIR) -I$(LIBAVCODEC_SOURCE_DIR)/libavutil
endif

OBJDIR = ./obj

ifeq ($(VERBOSE),)
Q_CC = @echo [CC] `echo $< | sed s^@OPALDIR@/^^` ;
Q_LD = @echo [LD] `echo $(PLUGIN) | sed s^@OPALDIR@/^^` ;
endif


$(OBJDIR)/%.o : %.cxx
	@mkdir -p $(OBJDIR) >/dev/null 2>&1
	$(Q_CC)$(CXX) -I../../../include $(EXTRACCFLAGS) $(STDCCFLAGS) $(OPTCCFLAGS) $(CFLAGS) -c $< -o $@

PLUGIN	= ./$(BASENAME)_video_pwplugin.$(PLUGINEXT)
STATIC	= ./lib$(BASENAME)_video_s.a

OBJECTS = $(addprefix $(OBJDIR)/,$(patsubst %.cxx,%.o,$(notdir $(SRCS))))

$(PLUGIN): $(OBJECTS)
ifeq (,$(findstring solaris,$(target_os)))
	$(Q_LD)$(CXX) $(LDSO) $(DL_LIBS) -o $@ $^
else	
	$(Q_LD)$(CXX) $(LDSO) $(DL_LIBS) $@ -o $@ $^
endif

install:
	mkdir -p $(DESTDIR)$(libdir)/$(VC_PLUGIN_DIR)
	install $(PLUGIN) $(DESTDIR)$(libdir)/$(VC_PLUGIN_DIR)

uninstall:
	rm -f $(DESTDIR)$(libdir)/$(VC_PLUGIN_DIR)/$(PLUGIN)

clean:
	rm -f $(OBJECTS) $(PLUGIN)

###########################################
