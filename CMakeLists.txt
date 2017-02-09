cmake_minimum_required(VERSION 3.2)

set(CMAKE_SYSTEM_NAME Linux)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall -Wno-unused-parameter -pedantic -fPIC -std=c++1y" CACHE STRING "" FORCE)

# Specify the cross compiler
set(CMAKE_CXX_COMPILER arm-frc-linux-gnueabi-g++)
INCLUDE(CMakeForceCompiler)
CMAKE_FORCE_CXX_COMPILER(arm-frc-linux-gnueabi-g++ GNU)
CMAKE_FORCE_C_COMPILER(arm-frc-linux-gnueabi-gcc GNU)
#set(CMAKE_CXX_COMPILER arm-frc-linux-gnueabi-g++)
set(CMAKE_CXX_FLAGS_DEBUG "-O0 -g3" CACHE STRING "" FORCE)
set(CMAKE_CXX_FLAGS_RELEASE "-O3 -g" CACHE STRING "" FORCE) # still want debugging for release?

project(pathfinder)

# search for programs in the build host directories
SET(CMAKE_FIND_ROOT_PATH_MODE_PROGRAM NEVER)
# for libraries and headers in the target directories
SET(CMAKE_FIND_ROOT_PATH_MODE_LIBRARY ONLY)
SET(CMAKE_FIND_ROOT_PATH_MODE_INCLUDE ONLY)

file(GLOB_RECURSE pathfinder_SOURCES src/*.cpp)
set_source_files_properties(${pathfinder_SOURCES} PROPERTIES LANGUAGE CXX)
add_library(pathfinder SHARED ${pathfinder_SOURCES})
target_include_directories(pathfinder PUBLIC include)

function(pathfinder_example name)
    add_executable(pathfinder_example_${name} examples/${name}.cpp)
    target_include_directories(pathfinder_example_${name} PUBLIC include)
    target_link_libraries(pathfinder_example_${name} pathfinder)
    set_target_properties(pathfinder_example_${name} PROPERTIES EXCLUDE_FROM_ALL TRUE EXCLUDE_FROM_DEFAULT_BUILD TRUE)
endfunction()

pathfinder_example(basic)
pathfinder_example(swerve)
pathfinder_example(tank)
