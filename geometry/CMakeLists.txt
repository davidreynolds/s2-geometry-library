include_directories("${CMAKE_SOURCE_DIR}/geometry")
include_directories("${CMAKE_SOURCE_DIR}/geometry/third_party/gtest")

set(BASE_HEADERS
        ${CMAKE_SOURCE_DIR}/geometry/base/basictypes.h
        ${CMAKE_SOURCE_DIR}/geometry/base/casts.h
        ${CMAKE_SOURCE_DIR}/geometry/base/commandlineflags.h
        ${CMAKE_SOURCE_DIR}/geometry/base/docid.h
        ${CMAKE_SOURCE_DIR}/geometry/base/int128.h
        ${CMAKE_SOURCE_DIR}/geometry/base/integral_types.h
        ${CMAKE_SOURCE_DIR}/geometry/base/logging.h
        ${CMAKE_SOURCE_DIR}/geometry/base/macros.h
        ${CMAKE_SOURCE_DIR}/geometry/base/port.h
        ${CMAKE_SOURCE_DIR}/geometry/base/scoped_ptr.h
        ${CMAKE_SOURCE_DIR}/geometry/base/stl_decl.h
        ${CMAKE_SOURCE_DIR}/geometry/base/stl_decl_msvc.h
        ${CMAKE_SOURCE_DIR}/geometry/base/stl_decl_osx.h
        ${CMAKE_SOURCE_DIR}/geometry/base/stringprintf.h
        ${CMAKE_SOURCE_DIR}/geometry/base/strtoint.h
        ${CMAKE_SOURCE_DIR}/geometry/base/template_util.h
        ${CMAKE_SOURCE_DIR}/geometry/base/type_traits.h)

set(BASE_SOURCE_FILES
        ${CMAKE_SOURCE_DIR}/geometry/base/int128.cc
        ${CMAKE_SOURCE_DIR}/geometry/base/logging.cc
        ${CMAKE_SOURCE_DIR}/geometry/base/stringprintf.cc
        ${CMAKE_SOURCE_DIR}/geometry/base/strtoint.cc)

add_library(google-base ${BASE_SOURCE_FILES} ${BASE_HEADERS})

set(STRINGS_SOURCE_FILES
        strings/ascii_ctype.cc
        strings/ascii_ctype.h
        strings/split.cc
        strings/split.h
        strings/stringprintf.cc
        strings/stringprintf.h
        strings/strutil.cc
        strings/strutil.h)

add_library(google-strings ${STRINGS_SOURCE_FILES} ${BASE_HEADERS})

set(CODING_SOURCE_FILES
        util/coding/coder.cc
        util/coding/coder.h
        util/coding/varint.cc
        util/coding/varint.h)

add_library(google-util-coding ${CODING_SOURCE_FILES} ${BASE_HEADERS})

set(MATH_SOURCE_FILES
        util/math/exactfloat/exactfloat.cc
        util/math/exactfloat/exactfloat.h
        util/math/mathlimits.cc
        util/math/mathlimits.h
        util/math/mathutil.cc
        util/math/mathutil.h
        util/math/matrix3x3-inl.h
        util/math/matrix3x3.h
        util/math/vector2-inl.h
        util/math/vector2.h
        util/math/vector3-inl.h
        util/math/vector3.h
        util/math/vector4-inl.h
        util/math/vector4.h)

add_library(google-util-math ${MATH_SOURCE_FILES})

set(S2_CELLID_SOURCE_FILES
        s1angle.cc
        s1angle.h
        s2.cc
        s2.h
        s2cellid.cc
        s2cellid.h
        s2latlng.cc
        s2latlng.h)

add_library(s2cellid ${S2_CELLID_SOURCE_FILES})

set(S2_SOURCE_FILES
        s1interval.cc
        s1interval.h
        s2cap.cc
        s2cap.h
        s2cell.cc
        s2cell.h
        s2cellunion.cc
        s2cellunion.h
        s2edgeindex.cc
        s2edgeindex.h
        s2edgeutil.cc
        s2edgeutil.h
        s2latlngrect.cc
        s2latlngrect.h
        s2loop.cc
        s2loop.h
        s2pointregion.cc
        s2pointregion.h
        s2polygon.cc
        s2polygon.h
        s2polygonbuilder.cc
        s2polygonbuilder.h
        s2polyline.cc
        s2polyline.h
        s2r2rect.cc
        s2r2rect.h
        s2region.cc
        s2region.h
        s2regioncoverer.cc
        s2regioncoverer.h
        s2regionintersection.cc
        s2regionintersection.h
        s2regionunion.cc
        s2regionunion.h)

add_library(s2 ${S2_SOURCE_FILES})

set(TEST_SOURCES
        r1interval_test.cc
        s1angle_test.cc
        s1interval_test.cc
        s2_test.cc
        s2cap_test.cc
        s2cell_test.cc
        s2cellid_test.cc
        s2cellunion_test.cc
        s2edgeindex_test.cc
        s2edgeutil_test.cc
        s2latlng_test.cc
        s2latlngrect_test.cc
        s2loop_test.cc
        s2pointregion_test.cc
        s2polygon_test.cc
        s2polygonbuilder_test.cc
        s2polyline_test.cc
        s2r2rect_test.cc
        s2regioncoverer_test.cc
        s2regionunion_test.cc)

set(S2_TESTING_SOURCES
        s2testing.cc
        s2testing.h)

add_library(s2testing ${S2_TESTING_SOURCES})

set(GTEST_SOURCES
        third_party/gtest/gtest/gtest-all.cc)
add_library(gtest ${GTEST_SOURCES})

add_executable(s2test ${TEST_SOURCES})
target_link_libraries(s2test LINK_PUBLIC gtest s2 s2testing s2cellid google-strings google-base google-util-math google-util-coding benchmark pthread ssl crypto gflags)