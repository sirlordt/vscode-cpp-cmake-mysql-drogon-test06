# version 3.11 or later of CMake or needed later for installing GoogleTest
# so let's require it now.
cmake_minimum_required(VERSION 3.11-3.18)

project(trantor)

add_library(trantor STATIC IMPORTED GLOBAL)

SET_TARGET_PROPERTIES(trantor PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/static/libtrantor.a)

# PUBLIC needed to make both lib_a.h and lib_a library available elsewhere in project
target_include_directories(${PROJECT_NAME} INTERFACE ${PROJECT_SOURCE_DIR}/include)
