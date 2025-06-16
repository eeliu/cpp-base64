cmake_minimum_required(VERSION 3.10)
project(base64)

add_library(base64 STATIC
    base64.h
    base64.cpp
)

target_include_directories(base64 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)
