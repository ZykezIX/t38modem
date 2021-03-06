#
# Makefile for Voicetronics VPB LID plugin
#
# Copyright (C) 2006 Post Increment, All Rights Reserved
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
# The Original Code is OPAL.
#
# The Initial Developer of the Original Code is Post Increment
#
# Contributor(s): ______________________________________.
#
# $Log: Makefile.in,v $
# Revision 1.5  2007/09/05 11:52:55  csoutheren
# Fix compilation on MacOSX
#
# Revision 1.4  2007/04/19 06:09:03  csoutheren
# Fixed plugin build system to use better detection for OSX
# Resolved 1636262 - Fix for audio plugins for OSX
#
# Revision 1.3  2006/10/10 01:42:54  csoutheren
# Applied patches from Ekiga bug 360869 - fix OpenSolaris build problems
# Thanks to Boying Lu
#
# Revision 1.2  2006/10/04 04:21:16  rjongbloed
# Fixed VPB plug in build under linux
#
# Revision 1.1  2006/10/02 13:30:53  rjongbloed
# Added LID plug ins
#
#

STDCCFLAGS = -fPIC
CXXFLAGS =  -Os 

LID_PLUGIN_DIR=opal-3.9.0/lid
prefix=/opt/t38modem
exec_prefix=${prefix}
libdir=${exec_prefix}/lib
target_os=linux-gnu

CC=gcc
CXX=g++
PLUGINEXT=so
SONAME	= vpb
SRCDIR	= ./src
OBJDIR	= ./obj
LDSO=-shared -Wl,-soname,$(SONAME)

SRCS	+= vpb.cpp

vpath	%.o $(OBJDIR)
vpath	%.cpp $(SRCDIR)

ifeq ($(VERBOSE),)
Q_CC = @echo [CC] `echo $< | sed s^@OPALDIR@/^^` ; 
Q_LD = @echo [LD] `echo $(PLUGIN) | sed s^@OPALDIR@/^^` ; 
endif


$(OBJDIR)/%.o : %.cpp
	@mkdir -p $(OBJDIR) >/dev/null 2>&1
	$(Q_CC)$(CXX) -I../../../include $(CXXFLAGS) $(STDCCFLAGS) $(OPTCCFLAGS) -c $< -o $@

PLUGIN	= ./vpb_lid_pwplugin.$(PLUGINEXT)

OBJECTS = $(addprefix $(OBJDIR)/,$(patsubst %.cpp,%.o,$(notdir $(SRCS))))

$(PLUGIN): $(OBJECTS)

ifeq (solaris,$(findstring solaris,$(target_os)))

	$(Q_LD)$(CC) $(LDSO) $@ -o $@ $^ $(EXTRALIBS)
else	
	$(Q_LD)$(CC) $(LDSO) -o $@ $^ $(EXTRALIBS)

endif	

install:
	mkdir -p $(DESTDIR)$(libdir)/$(LID_PLUGIN_DIR)
	install $(PLUGIN) $(DESTDIR)$(libdir)/$(LID_PLUGIN_DIR)

uninstall:
	rm -f $(DESTDIR)$(libdir)/$(LID_PLUGIN_DIR)/$(PLUGIN)

clean:
	rm -f $(OBJECTS) $(PLUGIN)

###########################################
