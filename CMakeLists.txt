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

set(INSTALL_LIB_DIR     lib     CACHE PATH "Library install directories")
set(INSTALL_BIN_DIR     bin     CACHE PATH "Executables install directories")
set(INSTALL_INCLUDE_DIR include CACHE PATH "Header files install directory")

# Make relative paths absilute
foreach(p LIB BIN INCLUDE CMAKE)
  set(var INSTALL_${p}_DIR)
  if(NOT IS_ABSOLUTE "${${var}}")
    set(${var} "${CMAKE_INSTALL_PREFIX}/${${var}}")
  endif()
endforeach()

# --------------------------------------------------------------------------- #
# Include sub-directories
# --------------------------------------------------------------------------- #
add_subdirectory(src)

