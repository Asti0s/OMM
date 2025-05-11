
option(NVRHI_INSTALL OFF)
set (SHADERMAKE_BIN_OUTPUT_PATH "${CMAKE_RUNTIME_OUTPUT_DIRECTORY}" CACHE STRING "")
option(BUILD_SHARED_LIBS "" OFF)
option(LZ4_BUILD_CLI "" OFF)

# lz4
set(LZ4_BUILD_CLI OFF)
set(LZ4_BUILD_LEGACY_LZ4C OFF)
add_subdirectory("lz4/build/cmake")
set_property(TARGET lz4_static PROPERTY FOLDER "External") 

# xxHash
set(XXHASH_BUILD_XXHSUM OFF)
add_subdirectory("xxHash/cmake_unofficial")
set_property(TARGET xxhash PROPERTY FOLDER "External") 

# stb
add_library(stb_lib INTERFACE)
target_include_directories(stb_lib INTERFACE $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/stb>)

# lz4
if (TARGET lz4)
	set_property(TARGET lz4 PROPERTY FOLDER "External") 
endif()
if (TARGET lz4_static)
	set_property(TARGET lz4_static PROPERTY FOLDER "External") 
endif()

# nvrhi
if (TARGET nvrhi)
		set_target_properties(nvrhi PROPERTIES FOLDER "External")
endif()

if (TARGET nvrhi_d3d12)
	set_target_properties(nvrhi_d3d12 PROPERTIES FOLDER "External")
endif()

# gtest
if(EXISTS ${CMAKE_CURRENT_SOURCE_DIR}/gtest AND OMM_ENABLE_TESTS)
	set(BUILD_GMOCK OFF)
	set(INSTALL_GTEST OFF)
	set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)
	add_subdirectory("gtest")
	set_target_properties("gtest_main" "gtest" PROPERTIES FOLDER "External")
endif()