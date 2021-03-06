if (WIN32)
    cmake_minimum_required (VERSION 3.15)
else()
    cmake_minimum_required(VERSION 3.10.2)
endif()
project (abNinjam VERSION 0.0.6)
set (PROJECT_DESCRIPTION "NINJAM client")

# External configuration CMake scripts
set (CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_CURRENT_SOURCE_DIR}/cmake")
include (abNinjamConfig)

option (ENABLE_LTO "Enable Link Time Optimization [default: ON]" ON)
option (ABNINJAM_VST "Enable VST plug-in build [default: ON]" ON)
option (ABNINJAM_LV2 "Enable LV2 plug-in build [default: ON]" ON)
option (ABNINJAM_USE_NJCLIENT "Use NJClient [default: ON]" ON)
option (ABNINJAM_USE_LIBLO "Use liblo [default: ON]" ON)
option (USE_STATIC_DEPENDENCIES "Use static dependencies [default: OFF]" OFF)

set(DEFAULT_CMAKE_FIND_LIBRARY_SUFFIXES ${CMAKE_FIND_LIBRARY_SUFFIXES})
set(STATIC_CMAKE_FIND_LIBRARY_SUFFIXES ".a" ".lib")
if (USE_STATIC_DEPENDENCIES)
    set(CMAKE_FIND_LIBRARY_SUFFIXES ${STATIC_CMAKE_FIND_LIBRARY_SUFFIXES})
endif()
message(STATUS "CMAKE_FIND_LIBRARY_SUFFIXES: " ${CMAKE_FIND_LIBRARY_SUFFIXES})

# Add the common library targets and sources
set (COMMON_BASEDIR "${CMAKE_CURRENT_SOURCE_DIR}/common")
set (NJCLIENT_BASEDIR "${CMAKE_CURRENT_SOURCE_DIR}/common/external/ninjam")
add_subdirectory (common)

if (ABNINJAM_VST)
    add_subdirectory (vst)
endif()

if (NOT MSVC AND ABNINJAM_LV2)
    add_subdirectory (lv2)
endif()

# Put it at the end so that the vst directories are registered
if (NOT MSVC)
    include(abNinjamUninstall)
endif()

show_build_info()
