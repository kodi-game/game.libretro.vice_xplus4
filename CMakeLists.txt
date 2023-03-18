cmake_minimum_required(VERSION 3.5)
project(game.libretro.vice_xplus4)

list(APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR})

find_package(Kodi REQUIRED)

# CMake on windows only searches for .lib libraries (static library or import library).
# The libretro game library is dynamically loaded so even if an import library would exists it's of no use.
if(CMAKE_SYSTEM_NAME STREQUAL Windows)
  set(_CMAKE_FIND_LIBRARY_SUFFIXES ${CMAKE_FIND_LIBRARY_SUFFIXES})
  set(CMAKE_FIND_LIBRARY_SUFFIXES ${CMAKE_SHARED_LIBRARY_SUFFIX})
endif()
find_library(VICE_XPLUS4_LIB NAMES vice_plus4_libretro${CMAKE_SHARED_LIBRARY_SUFFIX} PATH_SUFFIXES libretro)
if(CMAKE_SYSTEM_NAME STREQUAL Windows)
  set(CMAKE_FIND_LIBRARY_SUFFIXES ${_CMAKE_FIND_LIBRARY_SUFFIXES})
endif()

set(VICE_XPLUS4_CUSTOM_BINARY ${VICE_XPLUS4_LIB}
    ${PROJECT_NAME}${CMAKE_SHARED_LIBRARY_SUFFIX})

build_addon(${PROJECT_NAME} VICE_XPLUS4 "")

include(CPack)
