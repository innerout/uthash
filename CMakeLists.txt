include(cmake_utils/PreventInSouceBuilds.cmake.in)
cmake_minimum_required(VERSION 3.19)
project(
  uthash
  VERSION 2.3.0
  LANGUAGES C)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

# Disable clang-tidy to avoid warnings in CI when the project is used as a dependency
set(CMAKE_C_CLANG_TIDY "")

# Set a default build type if none was specified
set(default_build_type "Release")
if(EXISTS "${CMAKE_SOURCE_DIR}/.git")
  set(default_build_type "Debug")
endif()

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(
    STATUS
      "Setting build type to '${default_build_type}' as none was specified.")
  set(CMAKE_BUILD_TYPE
      "${default_build_type}"
      CACHE STRING "Choose the type of build." FORCE)
  # Set the possible values of build type for cmake-gui
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release"
                                               "MinSizeRel" "RelWithDebInfo")
endif()

include(GNUInstallDirs)

add_library(utarray INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/src/utarray.h)
target_include_directories(utarray INTERFACE
  $<INSTALL_INTERFACE:include/uthash>)

add_library(uthash INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/src/uthash.h)
target_include_directories(uthash INTERFACE
  $<INSTALL_INTERFACE:include/uthash>)

add_library(utlist INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/src/utlist.h)
target_include_directories(utlist INTERFACE
  $<INSTALL_INTERFACE:include/uthash>)

add_library(utringbuffer INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/src/utringbuffer.h)
target_include_directories(utringbuffer INTERFACE
  $<INSTALL_INTERFACE:include/uthash>)

add_library(utstack INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/src/utstack.h)
target_include_directories(utstack INTERFACE
  $<INSTALL_INTERFACE:include/uthash>)

add_library(utstring INTERFACE ${CMAKE_CURRENT_SOURCE_DIR}/src/utstring.h)
target_include_directories(utstring INTERFACE
  $<INSTALL_INTERFACE:include/uthash>)

install(
  TARGETS utarray uthash utlist utringbuffer utstack utstring
  EXPORT ${PROJECT_NAME}
  DESTINATION share/${PROJECT_NAME}/cmake
  ARCHIVE DESTINATION ${CMAKE_INSTALL_LIBDIR}
  LIBRARY DESTINATION ${CMAKE_INSTALL_LIBDIR}
  RUNTIME DESTINATION ${CMAKE_INSTALL_BINDIR})
set_target_properties( utarray uthash utlist utringbuffer utstack utstring
  PROPERTIES OUTPUT_NAME ${PROJECT_NAME} ${PROJECT_NAME}
  PROPERTIES PREFIX "")

file(GLOB HEADER_FILES ${CMAKE_CURRENT_SOURCE_DIR}/src/*.h)

export(TARGETS utarray uthash utlist utringbuffer utstack utstring FILE ${PROJECT_NAME}.cmake)
install(FILES ${HEADER_FILES} DESTINATION include/uthash)
