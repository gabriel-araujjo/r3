cmake_minimum_required(VERSION 3.0)
project(r3 VERSION 2.0.0)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake/Modules)
set(CMAKE_C_STANDARD 99)

option(ENABLE_TESTS "Whether the tests must be compiled and executed" ON)

# check for availability of certain headers
include(CheckIncludeFile)
CHECK_INCLUDE_FILE(dlfcn.h              HAVE_DLFCN_H)
CHECK_INCLUDE_FILE(inttypes.h           HAVE_INTTYPES_H)
CHECK_INCLUDE_FILE(jemalloc/jemalloc.h  HAVE_JEMALLOC_JEMALLOC_H)
CHECK_INCLUDE_FILE(memory.h             HAVE_MEMORY_H)
CHECK_INCLUDE_FILE(stdlib.h             HAVE_STDLIB_H)
CHECK_INCLUDE_FILE(string.h             HAVE_STRING_H)
CHECK_INCLUDE_FILE(stdbool.h            HAVE_STDBOOL_H)
CHECK_INCLUDE_FILE(strings.h            HAVE_STRINGS_H)
CHECK_INCLUDE_FILE(sys/stat.h           HAVE_SYS_STAT_H)
CHECK_INCLUDE_FILE(sys/time.h           HAVE_SYS_TIME_H)
CHECK_INCLUDE_FILE(unistd.h             HAVE_UNISTD_H)

# check availability of certain system functions
include(CheckSymbolExists)
CHECK_SYMBOL_EXISTS("gettimeofday"  "sys/time.h"    HAVE_GETTIMEOFDAY)
CHECK_SYMBOL_EXISTS("memset"        "string.h"      HAVE_MEMSET)
CHECK_SYMBOL_EXISTS("strchr"        "string.h"      HAVE_STRCHR)
CHECK_SYMBOL_EXISTS("strdup"        "string.h"      HAVE_STRDUP)
CHECK_SYMBOL_EXISTS("strndup"       "string.h"      HAVE_STRNDUP)
CHECK_SYMBOL_EXISTS("strnstr"       "string.h"      HAVE_STRNSTR)
CHECK_SYMBOL_EXISTS("strstr"        "string.h"      HAVE_STRSTR)

# check availability of certain types
include(CheckTypeSize)
CHECK_TYPE_SIZE(_Bool   _BOOL)

# generate the configuration file
set(CONFIG_INCLUDE_DIR "${CMAKE_CURRENT_BINARY_DIR}")
configure_file("config.h.in" "${CONFIG_INCLUDE_DIR}/config.h")

find_package(PCRE REQUIRED)
find_package(Check)

add_subdirectory(3rdparty)
add_subdirectory(src)

set(prefix ${CMAKE_INSTALL_PREFIX})
set(exec_prefix ${prefix})
set(includedir ${prefix}/include)
set(libdir ${prefix}/lib)
set(PACKAGE_VERSION ${PROJECT_VERSION})

# generate the package-config file
set(PC_DIR "${CMAKE_CURRENT_BINARY_DIR}")
configure_file(r3.pc.in "${PC_DIR}/r3.pc" @ONLY)
install(FILES "${PC_DIR}/r3.pc" DESTINATION lib/pkgconfig)

if(ENABLE_TESTS AND CHECK_FOUND)
  enable_testing()
  add_subdirectory(tests)
else(NOT ENABLE_TESTS OR CHECK_FOUND)
  message(STATUS "Skipping unit tests, Check library not found!")
endif(ENABLE_TESTS AND CHECK_FOUND)
