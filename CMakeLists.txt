cmake_minimum_required(VERSION 3.15.0)
project(sds LANGUAGES C)

set(CMAKE_C_STANDARD_REQUIRED ON)
set(CMAKE_C_EXTENSIONS OFF)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "No build type was specified. Setting build type to 'Debug'.")
  set(CMAKE_BUILD_TYPE "Debug" CACHE STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui.
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release" "MinSizeRel" "RelWithDebInfo")
endif()

macro(configure_compiler TARGET_NAME)
  target_compile_features(${TARGET_NAME} PUBLIC c_std_99)
  if(MSVC)
    target_compile_options(${TARGET_NAME} PRIVATE /W2 /WX)
  else()
    target_compile_options(${TARGET_NAME} PRIVATE -Wall -Wpedantic -Werror -Wunused-value)
  endif()
endmacro()

# Library

set(SDS_LIB_SOURCES sds.c sds.h sdsalloc.h)
add_library(sds STATIC ${SDS_LIB_SOURCES})
configure_compiler(sds)

# Test executable

add_executable(sds_test ${SDS_LIB_SOURCES})
configure_compiler(sds_test)
target_compile_definitions(sds_test PRIVATE SDS_TEST_MAIN)
