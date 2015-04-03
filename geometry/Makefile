# Please edit the following:
PYTHON_VER=2.7
SWIG_BINARY=swig
#GTEST_DIR=
GFLAGS_LNK=$(pkg-config --libs libgflags)
ZLIB_LNK=$(pkg-config --libs zlib)

#  ----- No more editing below -----

PYTHON_INC=-I/usr/include/python$(PYTHON_VER) -I/usr/lib/python$(PYTHON_VER)

DEBUG=-O -DNDEBUG
SYSCFLAGS=-fPIC
CCC=g++

GFLAGS_INC = $(pkg-config --cflags libgflags)
ZLIB_INC = $(pkg-config --cflags zlib)
CFLAGS= $(SYSCFLAGS) $(DEBUG) -I. -DARCH_K8 $(GFLAGS_INC) $(ZLIB_INC) \
        -Wno-deprecated -DS2_USE_EXACTFLOAT

# ----- OS Dependent -----
OS=$(shell uname -s)

ifeq ($(OS),Linux)
LD = gcc -shared
endif
ifeq ($(OS),Darwin) # Assume Mac Os X
LD = ld -arch x86_64 -bundle -flat_namespace -undefined suppress
endif

LDFLAGS=$(GFLAGS_LNK) $(ZLIB_LNK)

# Real targets

TESTS = \
	r1interval_test\
	s1angle_test\
	s1interval_test\
	s2_test\
	s2cap_test\
	s2cellid_test\
	s2cell_test\
	s2cellunion_test\
	s2latlngrect_test\
	s2pointregion_test\
	s2polygonbuilder_test\
	s2polyline_test\
	s2r2rect_test\
	s2regionunion_test

BENCHMARKS = \
	s2edgeindex_test\
	s2edgeutil_test\
	s2latlng_test\
	s2loop_test\
	s2polygon_test\
	s2regioncoverer_test

all: libs

test: all $(TESTS) $(BENCHMARKS)
	./r1interval_test
	./s1angle_test
	./s1interval_test
	./s2_test
	./s2cap_test
	./s2cellid_test
	./s2cell_test
	./s2cellunion_test
	./s2edgeindex_test --benchmark_filter=XXX
	./s2edgeutil_test --benchmark_filter=XXX
	./s2latlng_test --benchmark_filter=XXX
	./s2latlngrect_test
	./s2loop_test --benchmark_filter=XXX
	./s2pointregion_test
	./s2polygon_test --benchmark_filter=XXX
	./s2polygonbuilder_test
	./s2polyline_test
	./s2r2rect_test
	./s2regioncoverer_test --benchmark_filter=XXX
	./s2regionunion_test

bench: all $(TESTS) $(BENCHMARKS)
	./s2edgeindex_test --gtest_filter=XXX
	./s2edgeutil_test --gtest_filter=XXX
	./s2latlng_test --gtest_filter=XXX
	./s2loop_test --gtest_filter=XXX
	./s2polygon_test --gtest_filter=XXX
	./s2regioncoverer_test --gtest_filter=XXX

LIBS = \
	libgoogle-base.a\
	libgoogle-strings.a\
	libgoogle-util-math.a\
	libgoogle-util-coding.a\
	libgoogle-util-geometry-s2cellid.a\
	libgoogle-util-geometry-s2.a\
	libgoogle-util-geometry-s2testing.a

libgtest = libgtest.a

objs/gtest_main.o: $(GTEST_DIR)/src/gtest_main.cc
	$(CCC) $(DEBUG) -isystem $(GTEST_DIR)/include -I$(GTEST_DIR) \
	-c $(GTEST_DIR)/src/gtest_main.cc \
	-o objs/gtest_main.o

objs/gtest-all.o: $(GTEST_DIR)/src/gtest-all.cc
	$(CCC) $(DEBUG) -isystem $(GTEST_DIR)/include -I$(GTEST_DIR) \
	-c $(GTEST_DIR)/src/gtest-all.cc \
	-o objs/gtest-all.o

GTEST_OBJS = \
	objs/gtest-all.o

libgtest.a: $(GTEST_OBJS)
	ar rv libgtest.a $(GTEST_OBJS)

libs: $(LIBS) $(libgtest)

clean:
	rm -f *.a
	rm -f objs/*.o
	rm -f strings/objs/*.o
	rm -f util/coding/objs/*o
	rm -f util/math/objs/*.o
	rm -f base/objs/*.o
	rm -f $(TESTS)
	rm -f s2/*wrap*
	rm -f *.so

# base.

GOOGLE_BASE_LIB_OBJS = \
	base/objs/int128.o\
	base/objs/logging.o\
	base/objs/stringprintf.o\
	base/objs/strtoint.o

base/objs/int128.o:base/int128.cc
	$(CCC) $(CFLAGS) -c base/int128.cc -o base/objs/int128.o
base/objs/logging.o:base/logging.cc
	$(CCC) $(CFLAGS) -c base/logging.cc -o base/objs/logging.o
base/objs/stringprintf.o:base/stringprintf.cc
	$(CCC) $(CFLAGS) -c base/stringprintf.cc -o base/objs/stringprintf.o
base/objs/strtoint.o:base/strtoint.cc
	$(CCC) $(CFLAGS) -c base/strtoint.cc -o base/objs/strtoint.o

libgoogle-base.a: $(GOOGLE_BASE_LIB_OBJS)
	ar rv libgoogle-base.a $(GOOGLE_BASE_LIB_OBJS)

# strings

GOOGLE_STRINGS_LIB_OBJS = \
	strings/objs/ascii_ctype.o\
	strings/objs/split.o\
	strings/objs/stringprintf.o\
	strings/objs/strutil.o

strings/objs/ascii_ctype.o:strings/ascii_ctype.cc
	$(CCC) $(CFLAGS) -c strings/ascii_ctype.cc -o strings/objs/ascii_ctype.o
strings/objs/split.o:strings/split.cc
	$(CCC) $(CFLAGS) -c strings/split.cc -o strings/objs/split.o
strings/objs/stringprintf.o:strings/stringprintf.cc
	$(CCC) $(CFLAGS) -c strings/stringprintf.cc -o strings/objs/stringprintf.o
strings/objs/strutil.o:strings/strutil.cc
	$(CCC) $(CFLAGS) -c strings/strutil.cc -o strings/objs/strutil.o

libgoogle-strings.a: $(GOOGLE_STRINGS_LIB_OBJS)
	ar rv libgoogle-strings.a $(GOOGLE_STRINGS_LIB_OBJS)

# util/coding

GOOGLE_UTIL_CODING_LIB_OBJS = \
	util/coding/objs/coder.o\
	util/coding/objs/varint.o

util/coding/objs/coder.o:util/coding/coder.cc
	$(CCC) $(CFLAGS) -c util/coding/coder.cc -o util/coding/objs/coder.o
util/coding/objs/varint.o:util/coding/varint.cc
	$(CCC) $(CFLAGS) -c util/coding/varint.cc -o util/coding/objs/varint.o

libgoogle-util-coding.a: $(GOOGLE_UTIL_CODING_LIB_OBJS)
	ar rv libgoogle-util-coding.a $(GOOGLE_UTIL_CODING_LIB_OBJS)

# util/math

GOOGLE_UTIL_MATH_LIB_OBJS = \
	util/math/objs/mathutil.o\
	util/math/objs/mathlimits.o\
	util/math/objs/exactfloat.o

util/math/objs/mathutil.o:util/math/mathutil.cc
	$(CCC) $(CFLAGS) -c util/math/mathutil.cc -o util/math/objs/mathutil.o
util/math/objs/mathlimits.o:util/math/mathlimits.cc
	$(CCC) $(CFLAGS) -c util/math/mathlimits.cc -o util/math/objs/mathlimits.o
util/math/objs/exactfloat.o:util/math/exactfloat/exactfloat.cc
	$(CCC) $(CFLAGS) -c util/math/exactfloat/exactfloat.cc -o util/math/objs/exactfloat.o
#util/math/objs/exactfloat_test.o:util/math/exactfloat/exactfloat_test.cc
#	$(CCC) $(CFLAGS) -c util/math/exactfloat/exactfloat_test.cc -o util/math/objs/exactfloat_test.o
#util/math/objs/vector_unittest.o:util/math/vector_unittest.cc
#	$(CCC) $(CFLAGS) -c util/math/vector_unittest.cc -o util/math/objs/vector_unittest.o

libgoogle-util-math.a: $(GOOGLE_UTIL_MATH_LIB_OBJS)
	ar rv libgoogle-util-math.a $(GOOGLE_UTIL_MATH_LIB_OBJS)
#exactfloat_test: $(LIBS) objs/exactfloat_test.o
#	$(CCC) $(CFLAGS) $(LDFLAGS) objs/exactfloat_test.o $(LIBS) -o exactfloat_test
#vector_unittest: $(LIBS) objs/vector_unittest.o
#	$(CCC) $(CFLAGS) $(LDFLAGS) objs/vector_unittest.o $(LIBS) -o vector_unittest

# util/geometry

GOOGLE_UTIL_GEOMETRY_S2CELLID_LIB_OBJS = \
	objs/s1angle.o\
	objs/s2.o\
	objs/s2cellid.o\
	objs/s2latlng.o

GOOGLE_UTIL_GEOMETRY_S2_LIB_OBJS = \
	objs/s1interval.o\
	objs/s2cap.o\
	objs/s2cell.o\
	objs/s2cellunion.o\
	objs/s2edgeindex.o\
	objs/s2edgeutil.o\
	objs/s2latlngrect.o\
	objs/s2loop.o\
	objs/s2pointregion.o\
	objs/s2polygon.o\
	objs/s2polygonbuilder.o\
	objs/s2polyline.o\
	objs/s2r2rect.o\
	objs/s2region.o\
	objs/s2regioncoverer.o\
	objs/s2regionintersection.o\
	objs/s2regionunion.o

GOOGLE_UTIL_GEOMETRY_S2TESTING_LIB_OBJS = \
	objs/s2testing.o

objs/s1angle.o:s1angle.cc
	$(CCC) $(CFLAGS) -c s1angle.cc -o objs/s1angle.o
objs/s1interval.o:s1interval.cc
	$(CCC) $(CFLAGS) -c s1interval.cc -o objs/s1interval.o
objs/s2.o:s2.cc
	$(CCC) $(CFLAGS) -c s2.cc -o objs/s2.o
objs/s2cap.o:s2cap.cc
	$(CCC) $(CFLAGS) -c s2cap.cc -o objs/s2cap.o
objs/s2cell.o:s2cell.cc
	$(CCC) $(CFLAGS) -c s2cell.cc -o objs/s2cell.o
objs/s2cellid.o:s2cellid.cc
	$(CCC) $(CFLAGS) -c s2cellid.cc -o objs/s2cellid.o
objs/s2cellunion.o:s2cellunion.cc
	$(CCC) $(CFLAGS) -c s2cellunion.cc -o objs/s2cellunion.o
objs/s2edgeindex.o:s2edgeindex.cc
	$(CCC) $(CFLAGS) -c s2edgeindex.cc -o objs/s2edgeindex.o
objs/s2edgeutil.o:s2edgeutil.cc
	$(CCC) $(CFLAGS) -c s2edgeutil.cc -o objs/s2edgeutil.o
objs/s2latlng.o:s2latlng.cc
	$(CCC) $(CFLAGS) -c s2latlng.cc -o objs/s2latlng.o
objs/s2latlngrect.o:s2latlngrect.cc
	$(CCC) $(CFLAGS) -c s2latlngrect.cc -o objs/s2latlngrect.o
objs/s2loop.o:s2loop.cc
	$(CCC) $(CFLAGS) -c s2loop.cc -o objs/s2loop.o
objs/s2pointregion.o:s2pointregion.cc
	$(CCC) $(CFLAGS) -c s2pointregion.cc -o objs/s2pointregion.o
objs/s2polygon.o:s2polygon.cc
	$(CCC) $(CFLAGS) -c s2polygon.cc -o objs/s2polygon.o
objs/s2polygonbuilder.o:s2polygonbuilder.cc
	$(CCC) $(CFLAGS) -c s2polygonbuilder.cc -o objs/s2polygonbuilder.o
objs/s2polyline.o:s2polyline.cc
	$(CCC) $(CFLAGS) -c s2polyline.cc -o objs/s2polyline.o
objs/s2r2rect.o:s2r2rect.cc
	$(CCC) $(CFLAGS) -c s2r2rect.cc -o objs/s2r2rect.o
objs/s2region.o:s2region.cc
	$(CCC) $(CFLAGS) -c s2region.cc -o objs/s2region.o
objs/s2regioncoverer.o:s2regioncoverer.cc
	$(CCC) $(CFLAGS) -c s2regioncoverer.cc -o objs/s2regioncoverer.o
objs/s2regionintersection.o:s2regionintersection.cc
	$(CCC) $(CFLAGS) -c s2regionintersection.cc -o objs/s2regionintersection.o
objs/s2regionunion.o:s2regionunion.cc
	$(CCC) $(CFLAGS) -c s2regionunion.cc -o objs/s2regionunion.o
objs/s2testing.o:s2testing.cc
	$(CCC) $(CFLAGS) -c s2testing.cc -o objs/s2testing.o

libgoogle-util-geometry-s2cellid.a: $(GOOGLE_UTIL_GEOMETRY_S2CELLID_LIB_OBJS)
	ar rv libs2cellid.a $(GOOGLE_UTIL_GEOMETRY_S2CELLID_LIB_OBJS)

libgoogle-util-geometry-s2.a: $(GOOGLE_UTIL_GEOMETRY_S2_LIB_OBJS)
	ar rv libs2.a $(GOOGLE_UTIL_GEOMETRY_S2_LIB_OBJS)

libgoogle-util-geometry-s2testing.a: $(GOOGLE_UTIL_GEOMETRY_S2TESTING_LIB_OBJS)
	ar rv libs2testing.a $(GOOGLE_UTIL_GEOMETRY_S2TESTING_LIB_OBJS)

GOOGLE_GEOMETRY_LIBS = \
	libs2.a\
	libs2cellid.a\
	libgoogle-strings.a\
	libgoogle-base.a\
	libgoogle-util-math.a\
	libgoogle-util-coding.a

objs/%_test.o:%_test.cc
	$(CCC) $(CFLAGS) -isystem $(GTEST_DIR)/include \
	-c $< -o $@

r1interval_test:objs/r1interval_test.o objs/gtest_main.o
	$(CCC) -o r1interval_test objs/r1interval_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s1angle_test:objs/s1angle_test.o objs/gtest_main.o
	$(CCC) -o s1angle_test objs/s1angle_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s1interval_test:objs/s1interval_test.o objs/gtest_main.o
	$(CCC) -o s1interval_test objs/s1interval_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2_test:objs/s2_test.o objs/gtest_main.o
	$(CCC) -o s2_test objs/s2_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2cap_test:objs/s2cap_test.o objs/gtest_main.o
	$(CCC) -o s2cap_test objs/s2cap_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2cellid_test:objs/s2cellid_test.o objs/gtest_main.o
	$(CCC) -o s2cellid_test objs/s2cellid_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2cell_test:objs/s2cell_test.o objs/gtest_main.o
	$(CCC) -o s2cell_test objs/s2cell_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2cellunion_test:objs/s2cellunion_test.o objs/gtest_main.o
	$(CCC) -o s2cellunion_test objs/s2cellunion_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2edgeindex_test:objs/s2edgeindex_test.o
	$(CCC) -o s2edgeindex_test objs/s2edgeindex_test.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lbenchmark \
	-lgflags -lssl -lcrypto

s2edgeutil_test:objs/s2edgeutil_test.o
	$(CCC) -o s2edgeutil_test objs/s2edgeutil_test.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lbenchmark \
	-lgflags -lssl -lcrypto

s2latlng_test:objs/s2latlng_test.o
	$(CCC) -o s2latlng_test objs/s2latlng_test.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lbenchmark \
	-lgflags -lssl -lcrypto

s2latlngrect_test:objs/s2latlngrect_test.o
	$(CCC) -o s2latlngrect_test objs/s2latlngrect_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2loop_test:objs/s2loop_test.o
	$(CCC) -o s2loop_test objs/s2loop_test.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lbenchmark \
	-lgflags -lssl -lcrypto

s2pointregion_test:objs/s2pointregion_test.o
	$(CCC) -o s2pointregion_test objs/s2pointregion_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2polygon_test:objs/s2polygon_test.o
	$(CCC) -o s2polygon_test objs/s2polygon_test.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lbenchmark \
	-lgflags -lssl -lcrypto

s2polygonbuilder_test:objs/s2polygonbuilder_test.o
	$(CCC) -o s2polygonbuilder_test objs/s2polygonbuilder_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2polyline_test:objs/s2polyline_test.o
	$(CCC) -o s2polyline_test objs/s2polyline_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2r2rect_test:objs/s2r2rect_test.o
	$(CCC) -o s2r2rect_test objs/s2r2rect_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto

s2regioncoverer_test:objs/s2regioncoverer_test.o
	$(CCC) -o s2regioncoverer_test objs/s2regioncoverer_test.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lbenchmark \
	-lgflags -lssl -lcrypto

s2regionunion_test:objs/s2regionunion_test.o
	$(CCC) -o s2regionunion_test objs/s2regionunion_test.o objs/gtest_main.o \
	libgtest.a libs2testing.a \
	$(GOOGLE_GEOMETRY_LIBS) \
	-lgflags -lssl -lcrypto
