cmake_minimum_required(VERSION 2.8.0)

project(Quickplate)

set(CMAKE_MODULE_PATH
    "${CMAKE_SOURCE_DIR}/cmake"
)
include(OgreAddTargets)
add_subdirectory(src)


