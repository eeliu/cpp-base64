cmake_minimum_required(VERSION 3.10)
project(base64)

option(BASE64_ENABLE_WARNINGS "Enable strict warnings for base64 targets" OFF)

set(BASE64_WARNING_FLAGS
    -Werror
    -Wall
    -Wextra
    -pedantic
    -Wcast-align
    -Wcast-qual
    -Wdisabled-optimization
    -Wformat=2
    -Wmissing-include-dirs
    -Wmissing-declarations
    -Wold-style-cast
    -Woverloaded-virtual
    -Wredundant-decls
    -Wshadow
    -Wsign-promo
    -Wundef
    -Wno-unused
    -Wno-variadic-macros
    -Wno-parentheses
    -fdiagnostics-show-option
)

if(CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    list(APPEND BASE64_WARNING_FLAGS
        -Wctor-dtor-privacy
        -Winit-self
        -Wlogical-op
        -Wnoexcept
        -Wstrict-null-sentinel
        -Wstrict-overflow=5
    )
endif()

add_library(base64 STATIC
    base64.h
    base64.cpp
)

if(BASE64_ENABLE_WARNINGS)
    target_compile_options(base64 PRIVATE ${BASE64_WARNING_FLAGS})
endif()

target_include_directories(base64 PUBLIC
    $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
    $<INSTALL_INTERFACE:include>
)

include(CTest)

if(BUILD_TESTING)
    add_executable(base64-test test.cpp)
    target_link_libraries(base64-test PRIVATE base64)
    if(BASE64_ENABLE_WARNINGS)
        target_compile_options(base64-test PRIVATE ${BASE64_WARNING_FLAGS})
    endif()
    add_test(NAME base64-test COMMAND base64-test)
endif()
