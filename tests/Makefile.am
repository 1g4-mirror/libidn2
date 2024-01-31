# Copyright (C) 2011-2024 Simon Josefsson

# This program is free software: you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation, either version 3 of the License, or
# (at your option) any later version.

# This program is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.

# You should have received a copy of the GNU General Public License
# along with this program.  If not, see <http://www.gnu.org/licenses/>.

AM_CPPFLAGS = -I$(top_srcdir) -I$(top_srcdir)/lib -I$(top_builddir)/lib -I$(srcdir) -I$(top_builddir)
AM_CPPFLAGS += -I$(top_builddir)/gl -I$(top_srcdir)/gl
AM_CPPFLAGS += -DSRCDIR=\"$(srcdir)\"
AM_CPPFLAGS += -I$(top_srcdir)/unistring/ -I$(top_builddir)/unistring/
AM_CFLAGS = $(WERROR_CFLAGS) $(WERROR_TESTS_CFLAGS) $(WARN_CFLAGS)
AM_LDFLAGS = -no-install

AM_VALGRINDFLAGS = --suppressions=$(top_srcdir)/unistring/libunistring.valgrind

LDADD = ../lib/libidn2.la
LDADD += ../unistring/libunistring.la $(LTLIBUNISTRING)
LDADD += ../gl/libgnu.la

EXTRA_DIST = standalone.sh

ctests = test-version test-strerror test-locale test-tounicode
ctests += test-punycode test-compat-punycode
ctests += test-IdnaTest-inc test-IdnaTest-txt test-IdnaTestV2-txt
ctests += test-lookup test-register
if TEST_GLIBC
ctests += test-glibc
endif
check_PROGRAMS = $(ctests)

dist_check_SCRIPTS = test-idn2.sh

TESTS = $(ctests) $(dist_check_SCRIPTS)
TEST_EXTENSIONS = .sh
LOG_COMPILER = $(LOG_VALGRIND)
SH_LOG_COMPILER= $(SHELL) -x

AM_TESTS_ENVIRONMENT = EXEEXT="$(EXEEXT)" LOG_VALGRIND="$(LOG_VALGRIND)" LIBTOOL="$(LIBTOOL)"

IDNATEST_TXT = IdnaTest.txt
IDNATEST_INC = IdnaTest.inc

IDNATESTV2_TXT = IdnaTestV2.txt

BUILT_SOURCES = $(IDNATEST_TXT) $(IDNATEST_INC) $(IDNATESTV2_TXT)
MAINTAINERCLEANFILES = $(BUILT_SOURCES)
EXTRA_DIST += gen-utc-test.pl $(IDNATEST_TXT) $(IDNATEST_INC) $(IDNATESTV2_TXT)

$(IDNATEST_TXT):
	rm -f $(IDNATEST_TXT)
	wget https://www.unicode.org/Public/idna/10.0.0/IdnaTest.txt
	echo "35f344b624f9978ee5c484ab0182038e92a9f41c IdnaTest.txt" | sha1sum -c -

$(IDNATESTV2_TXT):
	rm -f $(IDNATESTV2_TXT)
	wget https://www.unicode.org/Public/idna/15.0.0/IdnaTestV2.txt
	echo "9d0193ad7ce877c76dbbfc8fa400dbf9221029d4 IdnaTestV2.txt" | sha1sum -c -

$(IDNATEST_INC): $(IDNATEST_TXT) $(srcdir)/gen-utc-test.pl
	$(srcdir)/gen-utc-test.pl < $< > $@
