cmake_minimum_required(VERSION 3.20)
project(my_matrix)

include(ExternalProject)

set(CMAKE-CXX-STANDARD 14)

include_directories(include/*)

add_subdirectory(src)
add_subdirectory(test)

ExternalProject_Add(
  googletest
  GIT_REPOSITORY "https://github.com/google/googletest.git"
  GIT_TAG "master"
  INSTALL_COMMAND ""
  LOG_DOWNLOAD ON
  LOG_CONFIGURE ON
  LOG_BUILD ON
)

ExternalProject_Get_Property(googletest source_dir)
set(GTEST_INCLUDE_DIR ${source_dir}/include)
ExternalProject_Get_Property(googletest binary_dir)
set(GTEST_LIBRARY_PATH ${binary_dir}/${CMAKE_FIND_LIBRARY_PREFIXES}gtest.a)
set(GTEST_LIBRARY gtest)
add_library(${GTEST_LIBRARY} UNKNOWN IMPORTED)
set_property(TARGET ${GTEST_LIBRARY} PROPERTY IMPORTED_LOCATION ${GTEST_LIBRARY_PATH} )
add_dependencies(${GTEST_LIBRARY} googletest)
