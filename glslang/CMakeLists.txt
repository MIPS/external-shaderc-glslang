if(WIN32)
    add_subdirectory(OSDependent/Windows)
elseif(UNIX)
    add_subdirectory(OSDependent/Unix)
else(WIN32)
    message("unknown platform")
endif(WIN32)

set(SOURCES
    MachineIndependent/glslang.y
    MachineIndependent/glslang_tab.cpp
    MachineIndependent/Constant.cpp
    MachineIndependent/InfoSink.cpp
    MachineIndependent/Initialize.cpp
    MachineIndependent/IntermTraverse.cpp
    MachineIndependent/Intermediate.cpp
    MachineIndependent/ParseContextBase.cpp
    MachineIndependent/ParseHelper.cpp
    MachineIndependent/PoolAlloc.cpp
    MachineIndependent/RemoveTree.cpp
    MachineIndependent/Scan.cpp
    MachineIndependent/ShaderLang.cpp
    MachineIndependent/SymbolTable.cpp
    MachineIndependent/Versions.cpp
    MachineIndependent/intermOut.cpp
    MachineIndependent/limits.cpp
    MachineIndependent/linkValidate.cpp
    MachineIndependent/parseConst.cpp
    MachineIndependent/reflection.cpp
    MachineIndependent/preprocessor/Pp.cpp
    MachineIndependent/preprocessor/PpAtom.cpp
    MachineIndependent/preprocessor/PpContext.cpp
    MachineIndependent/preprocessor/PpMemory.cpp
    MachineIndependent/preprocessor/PpScanner.cpp
    MachineIndependent/preprocessor/PpSymbols.cpp
    MachineIndependent/preprocessor/PpTokens.cpp
    MachineIndependent/propagateNoContraction.cpp
    GenericCodeGen/CodeGen.cpp
    GenericCodeGen/Link.cpp)

set(HEADERS
    Public/ShaderLang.h
    Include/arrays.h
    Include/BaseTypes.h
    Include/Common.h
    Include/ConstantUnion.h
    Include/InfoSink.h
    Include/InitializeGlobals.h
    Include/intermediate.h
    Include/PoolAlloc.h
    Include/ResourceLimits.h
    Include/revision.h
    Include/ShHandle.h
    Include/Types.h
    MachineIndependent/glslang_tab.cpp.h
    MachineIndependent/gl_types.h
    MachineIndependent/Initialize.h
    MachineIndependent/LiveTraverser.h
    MachineIndependent/localintermediate.h
    MachineIndependent/ParseHelper.h
    MachineIndependent/reflection.h
    MachineIndependent/RemoveTree.h
    MachineIndependent/Scan.h
    MachineIndependent/ScanContext.h
    MachineIndependent/SymbolTable.h
    MachineIndependent/Versions.h
    MachineIndependent/parseVersions.h
    MachineIndependent/propagateNoContraction.h
    MachineIndependent/preprocessor/PpContext.h
    MachineIndependent/preprocessor/PpTokens.h)

# This might be useful for making grammar changes:
#
# find_package(BISON)
# add_custom_command(OUTPUT ${CMAKE_CURRENT_SOURCE_DIR}/MachineIndependent/glslang_tab.cpp ${CMAKE_CURRENT_SOURCE_DIR}/MachineIndependent/glslang_tab.cpp.h
#                    COMMAND ${BISON_EXECUTABLE} --defines=${CMAKE_CURRENT_SOURCE_DIR}/MachineIndependent/glslang_tab.cpp.h -t ${CMAKE_CURRENT_SOURCE_DIR}/MachineIndependent/glslang.y -o ${CMAKE_CURRENT_SOURCE_DIR}/MachineIndependent/glslang_tab.cpp
#                    MAIN_DEPENDENCY MachineIndependent/glslang.y
#                    WORKING_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR})
# set(BISON_GLSLParser_OUTPUT_SOURCE ${CMAKE_CURRENT_SOURCE_DIR}/MachineIndependent/glslang_tab.cpp)

add_library(glslang STATIC ${BISON_GLSLParser_OUTPUT_SOURCE} ${SOURCES} ${HEADERS})
set_property(TARGET glslang PROPERTY FOLDER glslang)

if(WIN32)
    source_group("Public" REGULAR_EXPRESSION "Public/*")
    source_group("MachineIndependent" REGULAR_EXPRESSION "MachineIndependent/[^/]*")
    source_group("Include" REGULAR_EXPRESSION "Include/[^/]*")
    source_group("GenericCodeGen" REGULAR_EXPRESSION "GenericCodeGen/*")
    source_group("MachineIndependent\\Preprocessor" REGULAR_EXPRESSION "MachineIndependent/preprocessor/*")
endif(WIN32)

install(TARGETS glslang 
        ARCHIVE DESTINATION lib)
