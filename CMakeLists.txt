# GUIBuilder - A Matlab mixin class, with methods for creating interactive GUIs for objects.
#
# Mark J. Olah (mjo@cs.unm DOT edu)
# Copyright 2013-2019
# Licensed under the Apache License, Version 2.0
# https://www.apache.org/licenses/LICENSE-2.0
# See: LICENSE file

cmake_minimum_required( VERSION 3.9 )
project(GUIBuilder VERSION 0.1 LANGUAGES)

option(BUILD_TESTING "Enable testing" OFF)
option(OPT_DOC "Build documentation" OFF)
option(OPT_INSTALL_TESTING "Install testing executables" OFF)
option(OPT_EXPORT_BUILD_TREE "Configure the package so it is usable from the build tree.  Useful for development." OFF)
option(OPT_MATLAB_INSTALL_DISTRIBUTION_STARTUP "Install an additional copy of startup${PACKAGE_NAME}.m at the INSTALL_PREFIX root in addition to the normal directory.  Set only if this is the primary Matlab target for a standalone distribution archive." OFF)

message(STATUS "OPTION: BUILD_TESTING: ${BUILD_TESTING}")
message(STATUS "OPTION: OPT_DOC: ${OPT_DOC}")
message(STATUS "OPTION: OPT_INSTALL_TESTING: ${OPT_INSTALL_TESTING}")
message(STATUS "OPTION: OPT_EXPORT_BUILD_TREE: ${OPT_EXPORT_BUILD_TREE}")
message(STATUS "OPTION: OPT_MATLAB_INSTALL_DISTRIBUTION_STARTUP: ${OPT_MATLAB_INSTALL_DISTRIBUTION_STARTUP}")


### Dependencies
#Add UcommonCMakeModules git subpreo to path.
list(INSERT CMAKE_MODULE_PATH 0 ${CMAKE_CURRENT_LIST_DIR}/cmake/UncommonCMakeModules)
include(AddExternalDependency)
#CellFun - A functional programming oriented library for matlab CellArray manipulation.
set(CellFunURL https://github.com/markjolah/CellFun.git CACHE STRING "URL of CellFun library dependency")
add_external_dependency(NAME CellFun URL ${CellFunURL})

#Install matlab code, configure and install startup${PROJECT_NAME}.m and CMake Package Config files
include(${CMAKE_SOURCE_DIR}/cmake/CMakeMatlabPackageConfig/matlab_configure_install.cmake)
matlab_configure_install(PACKAGE_CONFIG_TEMPLATE "cmake/Templates/PackageConfig.cmake.in"
                         DEPENDENCY_STARTUP_M_LOCATIONS ${CellFun_MATLAB_STARTUP_M})

