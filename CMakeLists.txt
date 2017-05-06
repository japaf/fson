cmake_minimum_required (VERSION 3.0)

# --------------------------------------------------------------------------- #
# --> Outlaw "in-source" build
# --------------------------------------------------------------------------- #
set(CMAKE_DISABLE_SOURCE_CHANGES ON)
set(CMAKE_DISABLE_IN_SOURCE_BUILD ON)

project (fson VERSION 1.0 LANGUAGES Fortran)

# --------------------------------------------------------------------------- #
# --> Allow user to override the installation directories
# --------------------------------------------------------------------------- #

set(CMAKE_INSTALL_PREFIX $ENV{HOME})# Install in user-space

set(
  INSTALL_LIB_DIR lib/${PROJECT_NAME}
  CACHE PATH "Library install directories"
)
set(
  INSTALL_BIN_DIR bin
  CACHE PATH "Executables install directories"
)
set(
  INSTALL_INCLUDE_DIR include/${PROJECT_NAME}
  CACHE PATH "Header files install directory"
)
if(WIN32 AND NOT CYGWIN)
  set(DEF_INSTALL_CMAKE_DIR CMake)
else()
  set(DEF_INSTALL_CMAKE_DIR lib/cmake/fson)
endif()
set(
  INSTALL_CMAKE_DIR ${DEF_INSTALL_CMAKE_DIR} CACHE PATH
  "Installation directory for CMake files"
)

# Make relative paths absolute
foreach(p LIB BIN INCLUDE CMAKE)
  set(var INSTALL_${p}_DIR)
  if(NOT IS_ABSOLUTE "${${var}}")
    set(${var} "${CMAKE_INSTALL_PREFIX}/${${var}}")
  endif()
endforeach()

# set up include-directories
include_directories(
  "${PROJECT_SOURCE_DIR}"
  "${PROJECT_BINARY_DIR}"
)

# --------------------------------------------------------------------------- #
# Include sub-directories
# --------------------------------------------------------------------------- #
add_subdirectory(src)

# Add all targets to the build-tree export set
export(
  TARGETS fson
  FILE "${PROJECT_BINARY_DIR}/FSONTargets.cmake"
)

# Export the package for use from the build-tree
# (this registers the build-tree with a global CMake-registry)
export(PACKAGE fson)

# Create the FSONConfig.cmake and FSONConfigVersion files
file(
  RELATIVE_PATH REL_INCLUDE_DIR "${INSTALL_CMAKE_DIR}"
  "${INSTALL_INCLUDE_DIR}"
)
# ... for the build tree
set(CONF_INCLUDE_DIRS "${PROJECT_SOURCE_DIR}" "${PROJECT_BINARY_DIR}")
configure_file(
  FSONConfig.cmake.in
  "${PROJECT_BINARY_DIR}/FSONConfig.cmake" @ONLY)
# ... for the install tree
set(CONF_INCLUDE_DIRS "\${FSON_CMAKE_DIR}/${REL_INCLUDE_DIR}")
configure_file(
  FSONConfig.cmake.in
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/FSONConfig.cmake" @ONLY
)
# ... for both
configure_file(
  FSONConfigVersion.cmake.in
  "${PROJECT_BINARY_DIR}/FSONConfigVersion.cmake" @ONLY
)

# Install the FSONConfig.cmake and FSONConfigVersion.cmake
install(
  FILES
  "${PROJECT_BINARY_DIR}${CMAKE_FILES_DIRECTORY}/FSONConfig.cmake"
  "${PROJECT_BINARY_DIR}/FSONConfigVersion.cmake"
  DESTINATION "${INSTALL_CMAKE_DIR}"
)

# Install the export set for use with the install-tree
install(
  EXPORT FSONTargets
  DESTINATION "${INSTALL_CMAKE_DIR}"
)