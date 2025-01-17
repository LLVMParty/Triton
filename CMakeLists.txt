##
##  Copyright (C) - Triton
##
##  This program is under the terms of the BSD License.
##

##################################################################################### CMake libtriton

cmake_minimum_required(VERSION 3.15)

set(TRITON_ROOT "${CMAKE_CURRENT_LIST_DIR}")

# Read the build version
file(READ ${TRITON_ROOT}/.build_number BUILD_NUMBER)
set(VERSION_MAJOR 0)
set(VERSION_MINOR 9)
set(VERSION_BUILD ${BUILD_NUMBER})

project(triton VERSION ${VERSION_MAJOR}.${VERSION_MINOR})

# Define cmake options
include(CMakeDependentOption)

option(ASAN "Enable the ASAN linking" OFF)
option(GCOV "Enable code coverage" OFF)
option(KERNEL4 "Pin will run on a Linux's kernel v4" ON)
option(PINTOOL "Build Triton with the Pin tool as tracer" OFF)
option(BUILD_SHARED_LIBS "Build a shared library" ON)
cmake_dependent_option(PYTHON_BINDINGS "Enable Python bindings into the libtriton" ON BUILD_SHARED_LIBS OFF)
option(PYTHON_BINDINGS_AUTOCOMPLETE "Enable the generation of a triton_autocomplete module for IDE autocompletion" OFF)
option(Z3_INTERFACE "Use Z3 as SMT solver" ON)

#Enable ctest
include(CTest)

set(PYTHONPATH_CMD ${CMAKE_COMMAND} -E env PYTHONPATH=${CMAKE_CURRENT_BINARY_DIR}/src/libtriton/${CMAKE_CFG_INTDIR}/)

#[[if(PYTHON_BINDINGS)
    if(NOT PYTHON_VERSION)
        set(PYTHON_VERSION 3.6)
    endif()
    find_package(PythonInterp ${PYTHON_VERSION} REQUIRED)
    add_custom_target(test-python
        COMMAND ${PYTHONPATH_CMD} ${PYTHON_EXECUTABLE} -m unittest discover ${TRITON_ROOT}/src/testers/unittests
        DEPENDS python-triton
    )
else()
    add_custom_target(test-python
        COMMAND echo "No python test as python support is disabled"
    )
endif()]]#

if(PINTOOL AND NOT PYTHON_BINDINGS)
    MESSAGE(FATAL_ERROR "You can't have pintools without python binding.")
endif()

if(PINTOOL AND NOT "${PYTHON_VERSION_STRING}" MATCHES "2.7")
    MESSAGE(FATAL_ERROR "The Pin tracer needs Python 2.7.")
endif()

if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
  if(GCOV)
    add_custom_target(check
        COMMAND lcov --zerocounters --directory ${CMAKE_BINARY_DIR}/src/libtriton
        COMMAND lcov --capture --initial --directory ${CMAKE_BINARY_DIR}/src/libtriton --output-file app
        COMMAND ${CMAKE_COMMAND} --build ${CMAKE_BINARY_DIR} --target test-python
        COMMAND ${PYTHONPATH_CMD} ${TRITON_ROOT}/src/scripts/run_linux_test.sh
        COMMAND lcov --no-checksum --directory ${CMAKE_BINARY_DIR}/src/libtriton --capture --output-file coverage.info
        COMMAND lcov --remove coverage.info '/usr*' --remove coverage.info 'pintools*' --remove coverage.info 'examples*' -o coverage.info
        COMMAND genhtml coverage.info -o coverage
        COMMAND ${CMAKE_COMMAND} -E echo "-- Report generated in ${CMAKE_CURRENT_BINARY_DIR}/coverage/index.html"
    )
  else()
    # Special handling of Linux test to check if pin can be attached on other binaries.
    add_custom_target(check
        COMMAND ${PYTHONPATH_CMD} ${TRITON_ROOT}/src/scripts/run_linux_test.sh
        DEPENDS test-python
    )
  endif()
else()
    add_custom_target(check
        COMMAND ${PYTHONPATH_CMD} ctest --output-on-failure
        DEPENDS test-python
    )
endif()

# Defaut build type as Release
#if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
#    set(CMAKE_BUILD_TYPE Release)
#endif()

# Specific OSX POLICY
if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    if(POLICY CMP0025)
        cmake_policy(SET CMP0025 OLD) # report Apple's Clang as just Clang
    endif()
    if(POLICY CMP0042)
        cmake_policy(SET CMP0042 NEW) # MACOSX_RPATH
    endif()
endif()

# Get architecture
if(CMAKE_SIZEOF_VOID_P EQUAL 8)
    set(ARCHITECTURE amd64)
else()
    set(ARCHITECTURE i386)
endif()

# Find Python
if(PYTHON_BINDINGS)
    find_package(Python REQUIRED COMPONENTS Interpreter Development)
    add_definitions("-DPYTHON_LIBRARIES=\"${PYTHON_LIBRARIES}\"")
endif()

# Find Z3
if(Z3_INTERFACE)
    find_package(Z3 REQUIRED)
    set(TRITON_Z3_INTERFACE ON)
endif()

# Find capstone
find_package(capstone REQUIRED)

# Find boost
find_package(Boost 1.55.0 REQUIRED)

add_subdirectory(src)
add_subdirectory(doc)
