cmake_minimum_required (VERSION 3.0.2)
if (POLICY CMP0048)
  cmake_policy(SET CMP0048 NEW)
endif()

if (POLICY CMP0077)
  cmake_policy(SET CMP0077 NEW)
endif()

set (LIBMETAL_ROOT_DIR "${CMAKE_CURRENT_SOURCE_DIR}")

if (CMAKE_SYSTEM_PROCESSOR MATCHES riscv64)
  set (CMAKE_SYSTEM_NAME Linux)
  set (CMAKE_C_COMPILER ${CROSS_COMPILE}gcc)
  set (CMAKE_CXX_COMPILER ${CROSS_COMPILE}g++)
  set (LIBSYSFS_LIBRARY "${CMAKE_CURRENT_SOURCE_DIR}/third-party/libsysfs/riscv64/libsysfs.so")
  set (LIBRT_LIBRARIES "${CMAKE_CURRENT_SOURCE_DIR}/third-party/librt/riscv64/librt.so")
  include_directories(${CMAKE_CURRENT_SOURCE_DIR}/third-party/include/riscv64/)
endif()

list (APPEND CMAKE_MODULE_PATH
      "${CMAKE_CURRENT_SOURCE_DIR}/cmake"
      "${CMAKE_CURRENT_SOURCE_DIR}/cmake/modules"
      "${CMAKE_CURRENT_SOURCE_DIR}/cmake/platforms")

include (syscheck)
project (metal C)

include (CheckIncludeFiles)
include (CheckCSourceCompiles)
include (collect)
include (options)
include (depends)

foreach(_inc_path ${CMAKE_INCLUDE_PATH})
  collect (PROJECT_INC_DIRS "${_inc_path}")
endforeach()

enable_testing ()

add_subdirectory (lib)

if (WITH_TESTS)
  add_subdirectory (test)
endif (WITH_TESTS)

if (WITH_DOC)
  add_subdirectory (doc)
endif (WITH_DOC)

if (WITH_EXAMPLES)
  add_subdirectory (examples)
endif (WITH_EXAMPLES)

# vim: expandtab:ts=2:sw=2:smartindent
