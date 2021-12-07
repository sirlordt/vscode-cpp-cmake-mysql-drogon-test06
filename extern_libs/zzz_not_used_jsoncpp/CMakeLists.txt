# version 3.11 or later of CMake or needed later for installing GoogleTest
# so let's require it now.
cmake_minimum_required(VERSION 3.11-3.18)

project(jsoncpp)

find_package(jsoncpp REQUIRED)

get_target_property(JSON_INCLUDE_DIRECTORIES jsoncpp_lib INTERFACE_INCLUDE_DIRECTORIES)

message("JSONCPP_INCLUDE_DIR: ${JSON_INCLUDE_DIRECTORIES}")

#add_library(jsoncpp STATIC IMPORTED GLOBAL)

#SET_TARGET_PROPERTIES(jsoncpp PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/static/libjsoncpp.a)

#target_include_directories(${PROJECT_NAME} INTERFACE ${PROJECT_SOURCE_DIR}/include)

include_directories(${JSON_INCLUDE_DIRECTORIES})
