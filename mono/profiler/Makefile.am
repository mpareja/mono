if HAVE_ZLIB
Z_LIBS= -lz
else
Z_LIBS=
endif

INCLUDES = \
	-fexceptions -DMONO_USE_EXC_TABLES	\
	-I$(top_srcdir) 	\
	$(GLIB_CFLAGS)

if !DISABLE_PROFILER
if JIT_SUPPORTED
if PLATFORM_LINUX
bin_PROGRAMS = mprof-report
lib_LTLIBRARIES = libmono-profiler-cov.la libmono-profiler-aot.la libmono-profiler-iomap.la libmono-profiler-log.la
else
bin_PROGRAMS = mprof-report
lib_LTLIBRARIES = libmono-profiler-cov.la libmono-profiler-aot.la libmono-profiler-iomap.la libmono-profiler-log.la
endif
endif
endif

if HAVE_OPROFILE
# Do something that uses OPROFILE_CFLAGS and OPROFILE_LIBS
endif

libmono_profiler_cov_la_SOURCES = mono-cov.c
libmono_profiler_cov_la_LIBADD = $(top_builddir)/mono/mini/libmono-$(API_VER).la
libmono_profiler_aot_la_SOURCES = mono-profiler-aot.c
libmono_profiler_aot_la_LIBADD = $(top_builddir)/mono/mini/libmono-$(API_VER).la
#libmono_profiler_logging_la_SOURCES = mono-profiler-logging.c
#libmono_profiler_logging_la_LIBADD = $(top_builddir)/mono/mini/libmono-$(API_VER).la $(GLIB_LIBS)
libmono_profiler_iomap_la_SOURCES = mono-profiler-iomap.c
libmono_profiler_iomap_la_LIBADD = $(top_builddir)/mono/mini/libmono-$(API_VER).la $(GLIB_LIBS)
libmono_profiler_log_la_SOURCES = proflog.c
libmono_profiler_log_la_LIBADD = $(top_builddir)/mono/mini/libmono-$(API_VER).la $(Z_LIBS)

mprof_report_SOURCES = decode.c
mprof_report_LDADD = $(Z_LIBS)

PLOG_TESTS_SRC=test-alloc.cs test-busy.cs test-monitor.cs test-excleave.cs \
	test-heapshot.cs test-traces.cs
PLOG_TESTS=$(PLOG_TESTS_SRC:.cs=.exe)

MCS = $(RUNTIME) $(mcs_topdir)/class/lib/net_2_0/gmcs.exe -unsafe -nowarn:0162 -nowarn:0168 -nowarn:0219 -debug

%.exe: %.cs
	$(MCS) -out:$@ $<

testlog: $(PLOG_TESTS)
	perl $(srcdir)/ptestrunner.pl $(top_builddir)

check-local: testlog

EXTRA_DIST=utils.c utils.h proflog.h log-profiler.txt perf_event.h \
	$(PLOG_TESTS_SRC) ptestrunner.pl


