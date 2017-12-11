cmake_minimum_required(VERSION 2.8)
project(Lua)

if(WIN32)
    set (Lua_LIBRARIES lua )
	set ($LUA_C_FLAGS "-DLUA_COMPAT_BITLIB" CACHE STRING "Extra C Flags for Lua" )
elseif(UNIX)
	set ($LUA_C_FLAGS "-O3 -DLUA_COMPAT_BITLIB" CACHE STRING "Extra C Flags for Lua" )
    set(Lua_LIBRARIES lua dl m )
    set(Lua_EXEC_LIBRARIES readline)
    add_definitions(-DLUA_USE_LINUX)
endif()

# Extra C flags for Lua
set( CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${LUA_C_FLAGS}")

# define the lua source files
file(GLOB Lua_LIB_SRCS src/*.c)
file(GLOB Lua_Interpreter src/lua.c)
file(GLOB Lua_Compiler    src/luac.c)
list(REMOVE_ITEM Lua_LIB_SRCS ${Lua_Interpreter} ${Lua_Compiler})

# create the library
add_library (lua ${Lua_LIB_SRCS})

# create the lua executable and link it to the lib
add_executable (luaexec ${Lua_Interpreter})
target_link_libraries (luaexec ${Lua_LIBRARIES} ${Lua_EXEC_LIBRARIES})

# name the executable lua just like the library
set_target_properties(luaexec PROPERTIES OUTPUT_NAME lua)

# create the luac
add_executable (luac ${Lua_Compiler})
target_link_libraries (luac ${Lua_LIBRARIES} ${Lua_EXEC_LIBRARIES})
