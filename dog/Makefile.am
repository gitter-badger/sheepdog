#
# Copyright 2010 Red Hat, Inc.
#
# This program is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation.
#
# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with this program; see the file COPYING.  If not, write to
# the Free Software Foundation, 675 Mass Ave, Cambridge, MA 02139, USA.
#

MAINTAINERCLEANFILES	= Makefile.in

AM_CFLAGS		=

AM_CPPFLAGS		= -I$(top_builddir)/include -I$(top_srcdir)/include

sbin_PROGRAMS		= dog

dog_SOURCES		= farm/object_tree.c farm/sha1_file.c farm/snap.c \
			  farm/trunk.c farm/farm.c farm/slice.c \
			  dog.c common.c treeview.c vdi.c node.c cluster.c \
			  upgrade.c

if BUILD_TRACE
dog_SOURCES		+= trace.c
override CFLAGS         := $(subst -pg,,$(CFLAGS))
endif

if BUILD_NFS
dog_SOURCES		+= nfs.c
endif

dog_LDADD		= ../lib/libsd.a -lpthread
dog_DEPENDENCIES	= ../lib/libsd.a

noinst_HEADERS		= treeview.h dog.h farm/farm.h

EXTRA_DIST		=

if BUILD_EARTHQUAKE
dog_LDADD		+= -leq_embed
endif

install-exec-hook:
	if [ -z "${DESTDIR}" ];then $(LN_S) -f ${sbindir}/dog ${sbindir}/collie;fi

uninstall-hook:
	rm -f ${sbindir}/collie

all-local:
	@echo Built dog

clean-local:
	rm -f dog *.o gmon.out *.da *.bb *.bbg

# support for GNU Flymake
check-syntax:
	$(COMPILE) -fsyntax-only $(CHK_SOURCES)

check-style:
	@$(CHECK_STYLE) $(dog_SOURCES) $(noinst_HEADERS)

coverage:
	@lcov -d . -c -o dog.info
