cmake_minimum_required(VERSION 3.0)

project(cmake_crosscompiling_template)

if(CMAKE_VERSION VERSION_LESS "3.7.0")
    set(CMAKE_INCLUDE_CURRENT_DIR ON)
endif()

set(WORKSPACE_PATH "${CMAKE_SOURCE_DIR}/../") 

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} -std=c99")

add_definitions(-Wall -Wno-psabi)

aux_source_directory(src SOURCES)
aux_source_directory(include INCLUDES)

if(CMAKE_ARM_COMPILATION)
    link_directories(thirdparty/curl/arm)
else()
    link_directories(thirdparty/curl/x86)
endif()

add_executable(cmake_crosscompiling_template main.cc ${SOURCES} ${INCLUDES})
target_include_directories(cmake_crosscompiling_template PRIVATE include thirdparty/curl/include)
target_link_libraries(cmake_crosscompiling_template curl)

