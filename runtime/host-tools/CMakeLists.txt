# SPDX-License-Identifier: GPL-2.0-or-later
#
# Native (build-machine) tools needed while cross-compiling shadPS4 for aarch64:
#   - glslang-standalone: compiles the host shaders to SPIR-V headers
#   - protoc:             generates the shadNet protobuf sources
# The cross build cannot run its own aarch64 builds of these, so the runtime
# build script builds them here for the host and passes the paths back through
# BACHATA_HOST_GLSLANG / BACHATA_HOST_PROTOC.
cmake_minimum_required(VERSION 3.22)
project(bachata_host_tools C CXX)

set(CMAKE_CXX_STANDARD 20)
set(EXTERNALS "${CMAKE_CURRENT_LIST_DIR}/../../externals")

# glslang (same options as externals/CMakeLists.txt)
set(SKIP_GLSLANG_INSTALL ON CACHE BOOL "")
set(ENABLE_GLSLANG_BINARIES ON CACHE BOOL "" FORCE)
set(ENABLE_SPVREMAPPER OFF CACHE BOOL "")
set(ENABLE_CTEST OFF CACHE BOOL "")
set(ENABLE_HLSL OFF CACHE BOOL "")
set(BUILD_EXTERNAL OFF CACHE BOOL "")
set(ENABLE_OPT OFF CACHE BOOL "")
add_subdirectory("${EXTERNALS}/glslang" glslang EXCLUDE_FROM_ALL)

# abseil + protobuf (same options as externals/CMakeLists.txt)
add_subdirectory("${EXTERNALS}/abseil-cpp" abseil-cpp EXCLUDE_FROM_ALL)
set(protobuf_BUILD_TESTS              OFF CACHE INTERNAL "")
set(protobuf_BUILD_EXAMPLES           OFF CACHE INTERNAL "")
set(protobuf_BUILD_PROTOC_BINARIES    ON  CACHE INTERNAL "")
set(protobuf_INSTALL                  OFF CACHE INTERNAL "")
set(protobuf_WITH_ZLIB                OFF CACHE INTERNAL "")
set(protobuf_BUILD_SHARED_LIBS        OFF CACHE INTERNAL "")
set(protobuf_LOCAL_DEPENDENCIES_ONLY  ON  CACHE INTERNAL "")
set(protobuf_FORCE_FETCH_DEPENDENCIES OFF CACHE INTERNAL "")
add_subdirectory("${EXTERNALS}/protobuf" protobuf EXCLUDE_FROM_ALL)
