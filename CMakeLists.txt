cmake_minimum_required(VERSION 2.8.11)
project(s2_geometry_library)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS}")

find_library(gflags REQUIRED)
find_library(benchmark REQUIRED)

add_definitions(-DNDEBUG)
add_definitions(-DS2_USE_EXACTFLOAT)

add_subdirectory("geometry")