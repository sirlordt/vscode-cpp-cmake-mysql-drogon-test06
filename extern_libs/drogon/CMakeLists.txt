# version 3.11 or later of CMake or needed later for installing GoogleTest
# so let's require it now.
cmake_minimum_required(VERSION 3.11-3.18)

project(drogon)

#libjsoncpp-dev => 
#  /usr/include/jsoncpp (headers .h)
#  /usr/lib/x86_64-linux-gnu/libjsoncpp.a (static in windows .lib)
#  /usr/lib/x86_64-linux-gnu/libjsoncpp.so (dynamic in windows .dll)
#  /usr/lib/x86_64-linux-gnu/cmake/jsoncpp/jsoncppConfig.cmake (Read this file to undertand more what add to the project. jsoncpp_lib, jsoncpp_lib_static)
find_package(jsoncpp REQUIRED) #Search for the library in wide system

#Now the target jsoncpp_lib, jsoncpp_lib_static are available
#Now the variable INTERFACE_INCLUDE_DIRECTORIES are available

#Read the property INTERFACE_INCLUDE_DIRECTORIES created by find_package and save in local var JSONCPP_INCLUDE_DIRECTORIES.
#                   JSONCPP_INCLUDE_DIRECTORIES <- INTERFACE_INCLUDE_DIRECTORIES from target jsoncpp_lib
get_target_property(JSONCPP_INCLUDE_DIRECTORIES jsoncpp_lib INTERFACE_INCLUDE_DIRECTORIES)

#Uncomment the next line to check the value of ${JSON_INCLUDE_DIRECTORIES} variable. Must print "[cmake] JSONCPP_INCLUDE_DIR: /usr/include/jsoncpp"
#message("JSONCPP_INCLUDE_DIR: ${JSON_INCLUDE_DIRECTORIES}")

include_directories(${JSONCPP_INCLUDE_DIRECTORIES})

add_library(drogon STATIC IMPORTED GLOBAL)

set_target_properties(drogon PROPERTIES IMPORTED_LOCATION ${PROJECT_SOURCE_DIR}/static/libdrogon.a)

target_include_directories(${PROJECT_NAME} INTERFACE ${PROJECT_SOURCE_DIR}/include)

#Internal project libraries dendencies
target_link_libraries(drogon INTERFACE trantor)

#External project libraries system wide dependencies libjsoncpp-dev libbrotli-dev uuid-dev (apt-get install libjsoncpp-dev libbrotli-dev uuid-dev)
# 1 libjsoncpp-dev
target_link_libraries(drogon INTERFACE jsoncpp_lib_static) #The jsoncpp_lib_static add by find_package(jsoncpp REQUIRED), jsoncpp_lib is the *.so (.dll) file
# 2 libbrotli-dev (1)
target_link_libraries(drogon INTERFACE brotlienc)
# 3 libbrotli-dev (2)
target_link_libraries(drogon INTERFACE brotlidec)
# 4 libbrotli-dev (3): Warning this library must be in this EXACT order. Last later of brotlienc and brotlidec. To avoid linker error
target_link_libraries(drogon INTERFACE brotlicommon)
# 5 uuid-dev
target_link_libraries(drogon INTERFACE uuid)
