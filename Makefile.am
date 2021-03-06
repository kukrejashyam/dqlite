ACLOCAL_AMFLAGS = -I m4
AUTOMAKE_OPTIONS = foreign subdir-objects

lib_LTLIBRARIES =
check_PROGRAMS =
EXTRA_DIST =
noinst_HEADERS =
include_HEADERS =

EXTRA_DIST += dqlite.pc.in
pkgconfigdir = $(libdir)/pkgconfig
pkgconfig_DATA = dqlite.pc

AM_CFLAGS = -I$(top_srcdir)/include -Wall
AM_CFLAGS += $(SQLITE_CFLAGS) $(UV_CFLAGS) $(CO_CFLAGS) $(RAFT_CFLAGS)
AM_CFLAGS += $(CODE_COVERAGE_CFLAGS)
AM_LDFLAGS = $(SQLITE_LIBS) $(UV_LIBS) $(CO_LIBS) $(RAFT_LIBS) -lpthread
if DEBUG
  AM_CFLAGS += -g
else
  AM_CFLAGS += -O2
endif
if SANITIZE
  AM_CFLAGS += -fsanitize=address
endif

lib_LTLIBRARIES += libdqlite.la
libdqlite_la_LDFLAGS = $(AM_LDFLAGS) -version-info 0:1:0
libdqlite_la_SOURCES = \
  src/bind.c \
  src/client.c \
  src/command.c \
  src/conn.c \
  src/db.c \
  src/error.c \
  src/format.c \
  src/fsm.c \
  src/gateway.c \
  src/leader.c \
  src/lib/buffer.c \
  src/lib/transport.c \
  src/logger.c \
  src/message.c \
  src/metrics.c \
  src/config.c \
  src/query.c \
  src/registry.c \
  src/replication.c \
  src/request.c \
  src/response.c \
  src/server.c \
  src/stmt.c \
  src/transport.c \
  src/tuple.c \
  src/tx.c \
  src/vfs.c
include_HEADERS += include/dqlite.h

# Tests
check_PROGRAMS += \
  unit-test \
  integration-test

test_lib_SOURCES = \
  test/lib/endpoint.c \
  test/lib/fault.c \
  test/lib/fs.c \
  test/lib/heap.c \
  test/lib/logger.c \
  test/lib/munit.c \
  test/lib/server.c \
  test/lib/sqlite.c \
  test/lib/uv.c

unit_test_SOURCES = $(libdqlite_la_SOURCES) $(test_lib_SOURCES)
unit_test_SOURCES += \
  test/test_error.c \
  test/test_integration.c \
  test/unit/ext/test_co.c \
  test/unit/ext/test_uv.c \
  test/unit/lib/test_buffer.c \
  test/unit/lib/test_registry.c \
  test/unit/lib/test_serialize.c \
  test/unit/lib/test_transport.c \
  test/unit/test_command.c \
  test/unit/test_conn.c \
  test/unit/test_format.c \
  test/unit/test_gateway.c \
  test/unit/test_concurrency.c \
  test/unit/test_registry.c \
  test/unit/test_replication.c \
  test/unit/test_request.c \
  test/unit/test_tuple.c \
  test/unit/test_vfs.c \
  test/unit/main.c
unit_test_CFLAGS = $(AM_CFLAGS)
unit_test_CFLAGS += -I$(top_srcdir)/test
unit_test_CFLAGS += -DDQLITE_TEST -DMUNIT_TEST_NAME_LEN=70
unit_test_LDFLAGS = $(AM_LDFLAGS)

integration_test_SOURCES = $(libdqlite_la_SOURCES) $(test_lib_SOURCES)
integration_test_SOURCES += \
  test/integration/test_membership.c \
  test/integration/test_server.c \
  test/integration/main.c
integration_test_CFLAGS = $(AM_CFLAGS)
integration_test_CFLAGS += -I$(top_srcdir)/test
integration_test_CFLAGS += -DDQLITE_TEST -DMUNIT_TEST_NAME_LEN=70
integration_test_LDFLAGS =  -no-install $(AM_LDFLAGS)

TESTS = unit-test integration-test

if CODE_COVERAGE_ENABLED

include $(top_srcdir)/aminclude_static.am

CODE_COVERAGE_DIRECTORY=./src
CODE_COVERAGE_OUTPUT_DIRECTORY=coverage
CODE_COVERAGE_OUTPUT_FILE=coverage.info
CODE_COVERAGE_IGNORE_PATTERN="/usr/include/*"
CODE_COVERAGE_BRANCH_COVERAGE=1
CODE_COVERAGE_LCOV_OPTIONS=$(CODE_COVERAGE_LCOV_OPTIONS_DEFAULT) --rc lcov_excl_br_line="assert\("

clean-local: code-coverage-clean
distclean-local: code-coverage-dist-clean

endif # CODE_COVERAGE_ENABLED
