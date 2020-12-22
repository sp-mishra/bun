cmake_minimum_required(VERSION 3.9)

PROJECT(bun)

SET(CMAKE_CXX_STANDARD 20)
SET(CMAKE_CXX_STANDARD_REQUIRED ON)
SET(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_C_STANDARD_REQUIRED OFF)
set(CMAKE_C_EXTENSIONS OFF)
SET(CMAKE_VERBOSE_MAKEFILE ON)
SET(CMAKE_CXX_FLAGS_DISTRIBUTION "-E")
SET(CMAKE_C_FLAGS_DISTRIBUTION "-E")
#SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -E")
INCLUDE_DIRECTORIES(include)
INCLUDE_DIRECTORIES(/usr/local/include)
INCLUDE_DIRECTORIES(src/third_party/)
INCLUDE_DIRECTORIES(src/third_party/sqlite)
INCLUDE_DIRECTORIES(src/third_party/rapidjson/include)
INCLUDE_DIRECTORIES(src/third_party/spdlog/include)
INCLUDE_DIRECTORIES(src/third_party/fmt/include)
INCLUDE_DIRECTORIES(src/third_party/sqlpp11/include)
INCLUDE_DIRECTORIES(src/third_party/date/include)
INCLUDE_DIRECTORIES(src/third_party/iowow)
INCLUDE_DIRECTORIES(src/third_party/iowow/src)
INCLUDE_DIRECTORIES(src/third_party/iowow/src/kv)

FILE(GLOB
        SQLITE_C_SOURCE_FILES
        "src/third_party/sqlite/sqlite3.c"
        "src/third_party/sqlite/sqlite3.h"
        "src/third_party/sqlite/sqlite3ext.h"
        )

FILE(
        GLOB
        RAPIDJSON_SOURCE_FILES
        "src/third_party/rapidjson/include/rapidjson/*.h"
        "src/third_party/rapidjson/include/rapidjson/msinttypes/*.h"
        "src/third_party/rapidjson/include/rapidjson/internal/*.h"
        "src/third_party/rapidjson/include/rapidjson/error/*.h"
)

FILE(
        GLOB
        FMT_SOURCE_FILES
        "src/third_party/fmt/include/fmt/*.h"
        "src/third_party/fmt/src/*.cc"
)

FILE(
        GLOB
        SQLPP11_SOURCE_FILES
        "src/third_party/sqlpp11/include/sqlpp11/*.h"
        "src/third_party/sqlpp11/include/sqlpp11/detail/*.h"
        "src/third_party/sqlpp11/include/sqlpp11/data_types/*/*.h"
        "src/third_party/sqlpp11/include/sqlpp11/ppgen/colops/*.h"
        "src/third_party/sqlpp11/include/sqlpp11/ppgen/tblops/*.h"
        "src/third_party/sqlpp11/include/sqlpp11/ppgen/tools/*.h"
)

ADD_SUBDIRECTORY(src/third_party/iowow)

add_executable(
        bun
        ${SQLITE_C_SOURCE_FILES}
        ${RAPIDJSON_SOURCE_FILES}
        ${FMT_SOURCE_FILES}
        src/Main.cpp)

target_link_libraries(bun iowow)
if (WIN32)
    set(LEVELDB_PLATFORM_NAME LEVELDB_PLATFORM_WINDOWS)
    # TODO(cmumford): Make UNICODE configurable for Windows.
    add_definitions(-D_UNICODE -DUNICODE)
else (WIN32)
    set(LEVELDB_PLATFORM_NAME LEVELDB_PLATFORM_POSIX)
endif (WIN32)
option(LEVELDB_BUILD_TESTS "Build LevelDB's unit tests" OFF)
option(LEVELDB_BUILD_BENCHMARKS "Build LevelDB's benchmarks" OFF)
option(LEVELDB_INSTALL "Install LevelDB's header and library" OFF)

#set(SOURCE_FILES
#        src/sqlite/sqlite3.c
#        src/BoostHeaders.cpp
##        src/Bun.cpp
##        src/Generate.cpp
##        src/QueryExample.cpp
#        include/blib/bun/bun.hpp
#        include/blib/bun/CppTypeToSQLString.hpp
#        include/blib/bun/DbLogger.hpp
#        include/blib/bun/SimpleOID.hpp
#        include/blib/utils/MD5.hpp
#        include/blib/utils/Singleton.hpp
#        include/blib/utils/TypeUtils.hpp
#        include/blib/bun/PRef.hpp
#        include/blib/bun/PRefHelper.hpp
#        include/blib/bun/DbBackend.hpp
#        include/blib/bun/SqliteBackend.hpp)

#include_directories($ENV{BOOST_HEADER})
#include_directories($ENV{BLIB_HEADER})
#include_directories($ENV{THIRD_PARTY_HEADER})
#include_directories($ENV{INCLUDE_DIR})
#include_directories($ENV{SOCI_INCLUDE})

#link_directories($ENV{LIB_DIR})

#message("Print Dir: " $ENV{LIB_DIR})
#add_library(libsoci_sqlite3 STATIC IMPORTED)
#add_library(libsoci_core STATIC IMPORTED include/blib/bun/QueryHelper.hpp)

#set(LIBS ${LIBS} $ENV{LIB_DIR})
#find_library (SOCI_LIB NAME libsoci_sqlite3 HINTS $ENV{LIB_DIR})
#message("Lib = $ENV{LD_LIBRARY_PATH}")
#add_executable(bun ${SOURCE_FILES})
#target_link_libraries(bun $ENV{LIB_DIR}/libsoci_sqlite3.a $ENV{LIB_DIR}/libsoci_core.a)
