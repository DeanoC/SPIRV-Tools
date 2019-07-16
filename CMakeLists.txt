cmake_minimum_required(VERSION 3.13 FATAL_ERROR)
get_directory_property(hasParent PARENT_DIRECTORY)
if (NOT hasParent)
  set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/out_libs)
  set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/out_bin)
  set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/out_bin)
  set(LIB_BASE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/src/libs/)
  include(FetchContent)
  FetchContent_Declare(al2o3 GIT_REPOSITORY https://github.com/DeanoC/al2o3 GIT_TAG master)
  FetchContent_GetProperties(al2o3)
  if (NOT al2o3_POPULATED)
    FetchContent_Populate(al2o3)
    add_subdirectory(${al2o3_SOURCE_DIR} ${al2o3_BINARY_DIR})
  endif ()
endif ()

SET_MIN_VERSIONS()
set(LibName gfx_spirvtools)

project(${LibName})
set(Interface
        ../spirv-tools/instrument.hpp
        ../spirv-tools/libspirv.h
        ../spirv-tools/libspirv.hpp
        ../spirv-tools/linker.hpp
        ../spirv-tools/optimizer.hpp
        )
set(Src
        source/assembly_grammar.cpp
        source/assembly_grammar.h
        source/binary.cpp
        source/binary.h
        source/cfa.h
        source/CMakeLists.txt
        source/diagnostic.cpp
        source/diagnostic.h
        source/disassemble.cpp
        source/disassemble.h
        source/enum_set.h
        source/enum_string_mapping.cpp
        source/enum_string_mapping.h
        source/ext_inst.cpp
        source/ext_inst.h
        source/extensions.cpp
        source/extensions.h
        source/extinst.debuginfo.grammar.json
        source/extinst.spv-amd-gcn-shader.grammar.json
        source/extinst.spv-amd-shader-ballot.grammar.json
        source/extinst.spv-amd-shader-explicit-vertex-parameter.grammar.json
        source/extinst.spv-amd-shader-trinary-minmax.grammar.json
        source/instruction.h
        source/latest_version_glsl_std_450_header.h
        source/latest_version_opencl_std_header.h
        source/latest_version_spirv_header.h
        source/libspirv.cpp
        source/macro.h
        source/name_mapper.cpp
        source/name_mapper.h
        source/opcode.cpp
        source/opcode.h
        source/operand.cpp
        source/operand.h
        source/parsed_operand.cpp
        source/parsed_operand.h
        source/pch_source.cpp
        source/pch_source.h
        source/print.cpp
        source/print.h
        source/software_version.cpp
        source/spirv_constant.h
        source/spirv_definition.h
        source/spirv_endian.cpp
        source/spirv_endian.h
        source/spirv_fuzzer_options.cpp
        source/spirv_fuzzer_options.h
        source/spirv_optimizer_options.cpp
        source/spirv_optimizer_options.h
        source/spirv_reducer_options.cpp
        source/spirv_reducer_options.h
        source/spirv_target_env.cpp
        source/spirv_target_env.h
        source/spirv_validator_options.cpp
        source/spirv_validator_options.h
        source/table.cpp
        source/table.h
        source/text.cpp
        source/text.h
        source/text_handler.cpp
        source/text_handler.h

        source/util/bit_vector.cpp
        source/util/parse_number.cpp
        source/util/string_utils.cpp
        source/util/timer.cpp
        )

set(Deps
        gfx_spirvheaders
        )

ADD_LIB(${LibName} "${Interface}" "${Src}" "${Deps}")
