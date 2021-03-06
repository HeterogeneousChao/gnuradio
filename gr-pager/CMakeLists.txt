# Copyright 2011 Free Software Foundation, Inc.
#
# This file is part of GNU Radio
#
# GNU Radio is free software; you can redistribute it and/or modify
# it under the terms of the GNU General Public License as published by
# the Free Software Foundation; either version 3, or (at your option)
# any later version.
#
# GNU Radio is distributed in the hope that it will be useful,
# but WITHOUT ANY WARRANTY; without even the implied warranty of
# MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
# GNU General Public License for more details.
#
# You should have received a copy of the GNU General Public License
# along with GNU Radio; see the file COPYING.  If not, write to
# the Free Software Foundation, Inc., 51 Franklin Street,
# Boston, MA 02110-1301, USA.

########################################################################
# Setup dependencies
########################################################################
include(GrBoost)

########################################################################
# Register component
########################################################################
include(GrComponent)
GR_REGISTER_COMPONENT("gr-pager" ENABLE_GR_PAGER
    Boost_FOUND
    ENABLE_GNURADIO_RUNTIME
    ENABLE_GR_FILTER
    ENABLE_GR_ANALOG
)

GR_SET_GLOBAL(GR_PAGER_INCLUDE_DIRS
    ${CMAKE_CURRENT_SOURCE_DIR}/include
    ${CMAKE_CURRENT_BINARY_DIR}/include
)

GR_SET_GLOBAL(GR_PAGER_SWIG_INCLUDE_DIRS
    ${CMAKE_CURRENT_SOURCE_DIR}/swig
)

SET(GR_PKG_PAGER_EXAMPLES_DIR ${GR_PKG_DATA_DIR}/examples/pager)

########################################################################
# Begin conditional configuration
########################################################################
if(ENABLE_GR_PAGER)

########################################################################
# Setup CPack components
########################################################################
include(GrPackage)
CPACK_SET(CPACK_COMPONENT_GROUP_PAGER_DESCRIPTION "GNU Radio Pager Blocks")

CPACK_COMPONENT("pager_runtime"
    GROUP        "Pager"
    DISPLAY_NAME "Runtime"
    DESCRIPTION  "Runtime"
    DEPENDS      "runtime_runtime"
)

CPACK_COMPONENT("pager_devel"
    GROUP        "Pager"
    DISPLAY_NAME "Development"
    DESCRIPTION  "C++ headers, package config, import libraries"
    DEPENDS      "runtime_devel"
)

CPACK_COMPONENT("pager_python"
    GROUP        "Pager"
    DISPLAY_NAME "Python"
    DESCRIPTION  "Python modules for runtime; GRC xml files"
    DEPENDS      "runtime_python;pager_runtime"
)

CPACK_COMPONENT("pager_swig"
    GROUP        "Pager"
    DISPLAY_NAME "SWIG"
    DESCRIPTION  "SWIG development .i files"
    DEPENDS      "runtime_swig;pager_python;pager_devel"
)

########################################################################
# Add subdirectories
########################################################################
add_subdirectory(include/gnuradio/pager)
add_subdirectory(lib)
if(ENABLE_PYTHON)
    add_subdirectory(python/pager)
    add_subdirectory(swig)
    add_subdirectory(grc)
    add_subdirectory(apps)
endif(ENABLE_PYTHON)

########################################################################
# Create Pkg Config File
########################################################################
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/gnuradio-pager.pc.in
    ${CMAKE_CURRENT_BINARY_DIR}/gnuradio-pager.pc
@ONLY)

install(
    FILES ${CMAKE_CURRENT_BINARY_DIR}/gnuradio-pager.pc
    DESTINATION ${GR_LIBRARY_DIR}/pkgconfig
    COMPONENT "pager_devel"
)

endif(ENABLE_GR_PAGER)
