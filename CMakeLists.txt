cmake_minimum_required(VERSION 3.21.0)
project(verilog-netlist)
include(ExternalProject)
include(GNUInstallDirs)

# Boost
set(Boost_USE_STATIC_LIBS ON)
find_package(Boost 1.68.0 REQUIRED COMPONENTS
             unit_test_framework)

# Python
find_package(Python3 REQUIRED COMPONENTS
             Interpreter)

# Compiler
set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
add_compile_options(-Wall -pedantic)

ExternalProject_Add(SureLog
  SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/thirdparty/SureLog
  PREFIX SureLog
  CONFIGURE_COMMAND cmake <SOURCE_DIR> -DCMAKE_BUILD_TYPE=Release -DCMAKE_INSTALL_PREFIX=<INSTALL_DIR>
  BUILD_COMMAND make 
  BUILD_ALWAYS 1
  INSTALL_DIR ${CMAKE_INSTALL_PREFIX}
  INSTALL_COMMAND make install)

add_library(libsurelog STATIC IMPORTED GLOBAL)
add_library(libflatbuffers STATIC IMPORTED GLOBAL)
add_library(libantlr4-runtime STATIC IMPORTED GLOBAL)
add_library(libcapnp STATIC IMPORTED GLOBAL)
add_library(libkj STATIC IMPORTED GLOBAL)
add_library(libuhdm STATIC IMPORTED GLOBAL)

set_target_properties(libsurelog PROPERTIES
  IMPORTED_LOCATION ${CMAKE_INSTALL_PREFIX}/lib/surelog/libsurelog.a)

set_target_properties(libflatbuffers PROPERTIES
  IMPORTED_LOCATION ${CMAKE_INSTALL_PREFIX}/lib/surelog/libflatbuffers.a)

set_target_properties(libantlr4-runtime PROPERTIES
  IMPORTED_LOCATION ${CMAKE_INSTALL_PREFIX}/lib/surelog/libantlr4-runtime.a)

set_target_properties(libcapnp PROPERTIES
  IMPORTED_LOCATION ${CMAKE_INSTALL_PREFIX}/lib/uhdm/libcapnp.a)

set_target_properties(libkj PROPERTIES
  IMPORTED_LOCATION ${CMAKE_INSTALL_PREFIX}/lib/uhdm/libkj.a)

set_target_properties(libuhdm PROPERTIES
  IMPORTED_LOCATION ${CMAKE_INSTALL_PREFIX}/lib/uhdm/libuhdm.a)

include_directories(${CMAKE_INSTALL_PREFIX}/include/uhdm
                    ${CMAKE_INSTALL_PREFIX}/include/surelog
                    ${Boost_INCLUDE_DIRS})

add_executable(main main.cpp)
target_include_directories(main PUBLIC
  ${CMAKE_INSTALL_PREFIX}/include/surelog/surelog.h)
target_link_libraries(main libkj libcapnp libuhdm libflatbuffers libantlr4-runtime libsurelog ${Boost_LIBRARIES})

install(TARGETS main DESTINATION ${CMAKE_INSTALL_BINDIR})

#enable_testing()
#add_subdirectory(tests)
