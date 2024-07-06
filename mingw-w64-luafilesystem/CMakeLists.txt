cmake_minimum_required(VERSION 3.20)

project(LuaFileSystem C)

enable_testing()

include(MSYS2-GetLuaInfoFromPkgConfig-v1.0.0.cmake)

set(LFS_SOURCES "src/lfs.c" "src/lfs.h" "src/lfs.def")
set(LFS_SOURCE_TEST_FILE "tests/test.lua")
get_filename_component(LFS_TEST_FILE ${LFS_SOURCE_TEST_FILE} ABSOLUTE)

add_library(lfs SHARED ${LFS_SOURCES})
target_include_directories(lfs PRIVATE ${Lua_includedir})
target_link_libraries(lfs PRIVATE ${Lua_DLL})

set_target_properties(lfs
	PROPERTIES
	PREFIX ""
)

add_test(NAME lfs_test COMMAND ${Lua_EXE} ${LFS_TEST_FILE})

install(FILES $<TARGET_FILE:lfs>
	DESTINATION ${RELATIVE_Lua_INSTALL_CMOD})
