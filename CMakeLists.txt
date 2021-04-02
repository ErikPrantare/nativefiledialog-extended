cmake_minimum_required(VERSION 3.2)
project(nativefiledialog-extended)

set(NFD_GTK_VERSION "3" CACHE STRING "Select GTK version for NFD (either '3' or '4', default '3')")

set(nfd_PLATFORM Undefined)
if(WIN32)
  set(nfd_PLATFORM PLATFORM_WIN32)
elseif(APPLE)
  set(nfd_PLATFORM PLATFORM_MACOS)
elseif(UNIX AND NOT APPLE)
  if(NFD_GTK_VERSION STREQUAL "3")
    set(nfd_PLATFORM PLATFORM_LINUX_GTK3)
  elseif(NFD_GTK_VERSION STREQUAL "4")
    set(nfd_PLATFORM PLATFORM_LINUX_GTK4)
  else()
    message(FATAL_ERROR "Unknown GTK version")
endif()

message("nfd Platform: ${nfd_PLATFORM}")

set(nfd_COMPILER Undefined)
if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  set(nfd_COMPILER COMPILER_MSVC)
elseif(CMAKE_CXX_COMPILER_ID STREQUAL "GNU" OR CMAKE_CXX_COMPILER_ID STREQUAL "Clang" OR CMAKE_CXX_COMPILER_ID STREQUAL "AppleClang")
  set(nfd_COMPILER COMPILER_GNU)
endif()

message("nfd Compiler: ${nfd_COMPILER}")

set(CMAKE_CXX_STANDARD 17)

add_subdirectory(src)

option(NFD_BUILD_TESTS "Build tests for NFD (default 'OFF')" OFF)
if(${NFD_BUILD_TESTS})
    add_subdirectory(test)
endif()
