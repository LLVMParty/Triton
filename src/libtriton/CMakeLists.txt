if(${CMAKE_SYSTEM_NAME} MATCHES "Windows")
    set(PYTHON_SUFFIX .pyd)
else()
    set(PYTHON_SUFFIX .so)
endif()

# Define all source files
set(LIBTRITON_SOURCE_FILES
    api/api.cpp
    arch/architecture.cpp
    arch/arm/aarch64/aarch64Cpu.cpp
    arch/arm/aarch64/aarch64Semantics.cpp
    arch/arm/aarch64/aarch64Specifications.cpp
    arch/arm/arm32/arm32Cpu.cpp
    arch/arm/arm32/arm32Semantics.cpp
    arch/arm/arm32/arm32Specifications.cpp
    arch/arm/armOperandProperties.cpp
    arch/bitsVector.cpp
    arch/immediate.cpp
    arch/instruction.cpp
    arch/irBuilder.cpp
    arch/memoryAccess.cpp
    arch/operandWrapper.cpp
    arch/register.cpp
    arch/x86/x8664Cpu.cpp
    arch/x86/x86Cpu.cpp
    arch/x86/x86Semantics.cpp
    arch/x86/x86Specifications.cpp
    ast/ast.cpp
    ast/astContext.cpp
    ast/representations/astPythonRepresentation.cpp
    ast/representations/astRepresentation.cpp
    ast/representations/astSmtRepresentation.cpp
    callbacks/callbacks.cpp
    engines/solver/solverEngine.cpp
    engines/solver/solverModel.cpp
    engines/symbolic/pathConstraint.cpp
    engines/symbolic/pathManager.cpp
    engines/symbolic/symbolicEngine.cpp
    engines/symbolic/symbolicExpression.cpp
    engines/symbolic/symbolicSimplification.cpp
    engines/symbolic/symbolicVariable.cpp
    engines/taint/taintEngine.cpp
    modes/modes.cpp
    os/unix/syscallNumberToString.cpp
    utils/coreUtils.cpp
)

# Define all header files
set(LIBTRITON_HEADER_FILES
    includes/triton/aarch64.spec
    includes/triton/aarch64Cpu.hpp
    includes/triton/aarch64Semantics.hpp
    includes/triton/aarch64Specifications.hpp
    includes/triton/api.hpp
    includes/triton/archEnums.hpp
    includes/triton/architecture.hpp
    includes/triton/arm32.spec
    includes/triton/arm32Cpu.hpp
    includes/triton/arm32Semantics.hpp
    includes/triton/arm32Specifications.hpp
    includes/triton/armOperandProperties.hpp
    includes/triton/ast.hpp
    includes/triton/astContext.hpp
    includes/triton/astEnums.hpp
    includes/triton/astPythonRepresentation.hpp
    includes/triton/astRepresentation.hpp
    includes/triton/astRepresentationInterface.hpp
    includes/triton/astSmtRepresentation.hpp
    includes/triton/bitsVector.hpp
    includes/triton/callbacks.hpp
    includes/triton/callbacksEnums.hpp
    includes/triton/comparableFunctor.hpp
    includes/triton/coreUtils.hpp
    includes/triton/cpuInterface.hpp
    includes/triton/cpuSize.hpp
    includes/triton/dllexport.hpp
    includes/triton/exceptions.hpp
    includes/triton/externalLibs.hpp
    includes/triton/immediate.hpp
    includes/triton/instruction.hpp
    includes/triton/irBuilder.hpp
    includes/triton/memoryAccess.hpp
    includes/triton/modes.hpp
    includes/triton/modesEnums.hpp
    includes/triton/operandWrapper.hpp
    includes/triton/pathConstraint.hpp
    includes/triton/pathManager.hpp
    includes/triton/register.hpp
    includes/triton/semanticsInterface.hpp
    includes/triton/shortcutRegister.hpp
    includes/triton/solverEngine.hpp
    includes/triton/solverEnums.hpp
    includes/triton/solverInterface.hpp
    includes/triton/solverModel.hpp
    includes/triton/symbolicEngine.hpp
    includes/triton/symbolicEnums.hpp
    includes/triton/symbolicExpression.hpp
    includes/triton/symbolicSimplification.hpp
    includes/triton/symbolicVariable.hpp
    includes/triton/syscalls.hpp
    includes/triton/taintEngine.hpp
    includes/triton/tritonToZ3Ast.hpp
    includes/triton/tritonTypes.hpp
    includes/triton/unix.hpp
    includes/triton/x86.spec
    includes/triton/x8664Cpu.hpp
    includes/triton/x86Cpu.hpp
    includes/triton/x86Semantics.hpp
    includes/triton/x86Specifications.hpp
    includes/triton/z3Solver.hpp
    includes/triton/z3ToTritonAst.hpp
)

# Define all resource files
set(LIBTRITON_RESOURCE_FILES
    includes/triton/aarch64.spec
    includes/triton/arm32.spec
    includes/triton/x86.spec
    includes/triton/version.hpp.in
    includes/triton/config.hpp.in
)

if(Z3_INTERFACE)
    set(Z3_INTERFACE_SOURCE_FILES
        ast/z3/tritonToZ3Ast.cpp
        ast/z3/z3ToTritonAst.cpp
        engines/solver/z3/z3Solver.cpp
    )
else()
    set(Z3_INTERFACE_SOURCE_FILES)
endif()

if(PYTHON_BINDINGS)
    set(LIBTRITON_PYTHON_SOURCE_FILES
        bindings/python/init.cpp
        bindings/python/modules/tritonCallbacks.cpp
        bindings/python/namespaces/initArchNamespace.cpp
        bindings/python/namespaces/initAstNodeNamespace.cpp
        bindings/python/namespaces/initAstRepresentationNamespace.cpp
        bindings/python/namespaces/initCallbackNamespace.cpp
        bindings/python/namespaces/initConditionsNamespace.cpp
        bindings/python/namespaces/initCpuSizeNamespace.cpp
        bindings/python/namespaces/initExtendNamespace.cpp
        bindings/python/namespaces/initModeNamespace.cpp
        bindings/python/namespaces/initOpcodesNamespace.cpp
        bindings/python/namespaces/initOperandNamespace.cpp
        bindings/python/namespaces/initPrefixesNamespace.cpp
        bindings/python/namespaces/initRegNamespace.cpp
        bindings/python/namespaces/initShiftsNamespace.cpp
        bindings/python/namespaces/initSolverNamespace.cpp
        bindings/python/namespaces/initSymbolicNamespace.cpp
        bindings/python/namespaces/initSyscallNamespace.cpp
        bindings/python/namespaces/initVersionNamespace.cpp
        bindings/python/objects/pyAstContext.cpp
        bindings/python/objects/pyAstNode.cpp
        bindings/python/objects/pyBitsVector.cpp
        bindings/python/objects/pyImmediate.cpp
        bindings/python/objects/pyInstruction.cpp
        bindings/python/objects/pyMemoryAccess.cpp
        bindings/python/objects/pyPathConstraint.cpp
        bindings/python/objects/pyRegister.cpp
        bindings/python/objects/pySolverModel.cpp
        bindings/python/objects/pySymbolicExpression.cpp
        bindings/python/objects/pySymbolicVariable.cpp
        bindings/python/objects/pyTritonContext.cpp
        bindings/python/pyXFunctions.cpp
        bindings/python/utils.cpp
    )
    set(LIBTRITON_PYTHON_HEADER_FILES
        includes/triton/py3c_compat.h
        includes/triton/pythonBindings.hpp
        includes/triton/pythonObjects.hpp
        includes/triton/pythonUtils.hpp
        includes/triton/pythonXFunctions.hpp
    )
else()
    set(LIBTRITON_PYTHON_SOURCE_FILES)
    set(LIBTRITON_PYTHON_HEADER_FILES)
endif()

# Triton have to generate a syscalls table from the kernel source
# This following code tries to find the unistd_64.h or unistd_32.h header depending on the architecture.
if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    if(EXISTS "/usr/include/*unistd_32.h")
       FILE(GLOB_RECURSE syscalls32_table_files /usr/include/*unistd_32.h)
    else()
       FILE(GLOB_RECURSE syscalls32_table_files /usr/include/*unistd.h)
    endif()

    if(EXISTS "/usr/include/*unistd_32.h")
        FILE(GLOB_RECURSE syscalls64_table_files /usr/include/*unistd_64.h)
    else()
       FILE(GLOB_RECURSE syscalls64_table_files /usr/include/*unistd.h)
    endif()
endif()

macro(find_osx_syscall path)
    if(NOT OSX_SYSCALL_H AND EXISTS ${path})
        set(OSX_SYSCALL_H ${path})
    endif()
endmacro()

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    find_osx_syscall("/usr/include/sys/syscall.h")
    find_osx_syscall("/Applications/Xcode.app/Contents/Developer/Platforms/MacOSX.platform/Developer/SDKs/MacOSX.sdk/usr/include/sys/syscall.h")
    if(NOT OSX_SYSCALL_H OR NOT EXISTS ${OSX_SYSCALL_H})
        message(FATAL_ERROR "syscall.h not found!")
    endif()
    FILE(GLOB_RECURSE syscalls64_table_files ${OSX_SYSCALL_H})
endif()

# Create syscall generted file directory
file(MAKE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/os/unix/)

set(EXTRACT_SYSCALL_SCRIPT ${TRITON_ROOT}/src/scripts/extract_syscall.py)

macro(gen_syscall bits)
    # If the unistd_64.h or syscall.h is not found, we exit
    if(NOT syscalls${bits}_table_files)
        message(FATAL_ERROR "unistd_${bits}.h or syscall.h is missing")
    endif()
    LIST(GET syscalls${bits}_table_files 0 syscalls${bits}_table_file)

    set(OUT_SYSCALL${bits} ${CMAKE_CURRENT_BINARY_DIR}/os/unix/syscalls${bits}.cpp)

    # We generate the syscalls${bits}.cpp from the unistd_${bits}.h or syscall.h.
    # Added python after COMMAND since to be sure that if ${TRITON_ROOT}/src/scripts/extract_syscall.py doesn't have X rights it gets executed
    add_custom_command(
        OUTPUT ${OUT_SYSCALL${bits}}
        COMMAND ${PYTHON_EXECUTABLE} ${EXTRACT_SYSCALL_SCRIPT} ${syscalls${bits}_table_file} ${bits} > ${OUT_SYSCALL${bits}}
        MAIN_DEPENDENCY ${EXTRACT_SYSCALL_SCRIPT}
        DEPENDS ${syscalls${bits}_table_file}
    )
    add_custom_target(gen-syscall${bits} DEPENDS ${OUT_SYSCALL${bits}})
    set(LIBTRITON_SOURCE_FILES ${LIBTRITON_SOURCE_FILES} ${OUT_SYSCALL${bits}})
endmacro()

# Note: Windows ?!
if(${CMAKE_SYSTEM_NAME} MATCHES "Linux" OR ${CMAKE_SYSTEM_NAME} MATCHES "Darwin")
    gen_syscall(64)
else()
    add_custom_target(gen-syscall64
      COMMAND echo "No syscall64 to extract on your plateform"
    )
endif()

# Syscall 32 is only available for linux users
if(${CMAKE_SYSTEM_NAME} MATCHES "Linux")
    gen_syscall(32)
else()
    add_custom_target(gen-syscall32
      COMMAND echo "No syscall32 to extract on your plateform"
    )
endif()

# We generate the version numbers information
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/includes/triton/version.hpp.in
    ${CMAKE_CURRENT_BINARY_DIR}/includes/triton/version.hpp
    IMMEDIATE @ONLY
)

# We generate the config header based on configuation of CMake
configure_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/includes/triton/config.hpp.in
    ${CMAKE_CURRENT_BINARY_DIR}/includes/triton/config.hpp
    IMMEDIATE @ONLY
)

# Define library's properties
add_library(
    triton
    ${LIBTRITON_SOURCE_FILES} ${LIBTRITON_HEADER_FILES} ${LIBTRITON_RESOURCE_FILES}
    ${Z3_INTERFACE_SOURCE_FILES}
    ${LIBTRITON_PYTHON_SOURCE_FILES} ${LIBTRITON_PYTHON_HEADER_FILES}
)
add_dependencies(check triton)

set_target_properties(triton PROPERTIES PUBLIC_HEADER "${LIBTRITON_HEADER_FILES};${CMAKE_CURRENT_BINARY_DIR}/includes/triton/version.hpp;${CMAKE_CURRENT_BINARY_DIR}/includes/triton/config.hpp")

target_include_directories(triton PUBLIC
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_LIST_DIR}/includes>"
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_BINARY_DIR}/includes>"
)
add_dependencies(triton gen-syscall32 gen-syscall64)

# Link Triton's dependencies
target_link_libraries(triton PUBLIC
    Boost::headers
    z3::libz3
    capstone::capstone
    ${LIBTRITON_OTHER_LIBS}
)

if(PYTHON_BINDINGS)
    target_link_libraries(triton PRIVATE
        Python::Python
    )
endif()

# Workaround to allow building 'namespace linux' (defined by -std=gnu++11)
if(NOT MSVC AND (CMAKE_CXX_COMPILER_ID MATCHES "Clang" OR CMAKE_CXX_COMPILER_ID STREQUAL "GNU"))
    target_compile_options(triton PRIVATE -std=c++11)
endif()

if(WIN32 AND BUILD_SHARED_LIBS)
    target_compile_definitions(triton PUBLIC TRITON_BUILDING_DLL)
endif()

# Installation
install(
        EXPORT tritonTargets
        DESTINATION lib/cmake/triton
        NAMESPACE triton::
    )
    install(
        TARGETS triton
        EXPORT tritonTargets
        PUBLIC_HEADER DESTINATION include/triton
        INCLUDES DESTINATION include
    )

    include(CMakePackageConfigHelpers)
    configure_package_config_file(
        Config.cmake.in
        "${PROJECT_BINARY_DIR}/tritonConfig.cmake"
        INSTALL_DESTINATION lib/cmake/triton
        NO_SET_AND_CHECK_MACRO
        NO_CHECK_REQUIRED_COMPONENTS_MACRO
    )

    write_basic_package_version_file("${PROJECT_BINARY_DIR}/tritonConfigVersion.cmake"
        COMPATIBILITY SameMajorVersion
    )

    install(
        FILES
            "${PROJECT_BINARY_DIR}/tritonConfig.cmake"
            "${PROJECT_BINARY_DIR}/tritonConfigVersion.cmake"
        DESTINATION lib/cmake/triton
    )

# Add the install targets
#install (TARGETS triton DESTINATION lib)
#install (DIRECTORY ${TRITON_ROOT}/src/libtriton/includes/triton DESTINATION include)
#install (DIRECTORY ${CMAKE_BINARY_DIR}/src/libtriton/includes/triton DESTINATION include)

# Install Python bindings
#[[
if(PYTHON_BINDINGS)
    add_custom_target(python-triton ALL
        COMMAND ${CMAKE_COMMAND} -E copy "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_CFG_INTDIR}/${CMAKE_SHARED_MODULE_PREFIX}triton${CMAKE_SHARED_LIBRARY_SUFFIX}" "${CMAKE_CURRENT_BINARY_DIR}/${CMAKE_CFG_INTDIR}/triton${PYTHON_SUFFIX}"
        DEPENDS triton
    )

    execute_process (COMMAND ${PYTHON_EXECUTABLE} -c "from sysconfig import get_path; print(get_path('platlib'))" OUTPUT_VARIABLE PYTHON_SITE_PACKAGES OUTPUT_STRIP_TRAILING_WHITESPACE)
    if(${CMAKE_SYSTEM_NAME} MATCHES "Windows" AND NOT ${CMAKE_CXX_COMPILER_ID} MATCHES "GNU")
        install (FILES ${CMAKE_CURRENT_BINARY_DIR}/\${CMAKE_INSTALL_CONFIG_NAME}/triton${PYTHON_SUFFIX} DESTINATION ${PYTHON_SITE_PACKAGES})
    else()
        install (FILES ${CMAKE_CURRENT_BINARY_DIR}/triton${PYTHON_SUFFIX} DESTINATION ${PYTHON_SITE_PACKAGES})
    endif()
endif()
]]#