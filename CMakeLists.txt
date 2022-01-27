cmake_minimum_required(VERSION 3.5)

project(minunit C)

set(CMAKE_C_STANDARD 99)

find_program(CLANG_TIDY NAMES clang-tidy)
if(NOT CLANG_TIDY)
    message(WARNING "Clang tidy not found")
else()
    set(CMAKE_C_CLANG_TIDY ${CLANG_TIDY})
endif()

add_library(${PROJECT_NAME} INTERFACE)
target_include_directories(${PROJECT_NAME}
    INTERFACE
        $<INSTALL_INTERFACE:include>
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/include>)

if(MSVC)
    target_compile_options(${PROJECT_NAME} INTERFACE /W2)
else()
    target_compile_options(${PROJECT_NAME} INTERFACE -Wall -Wpedantic -Werror)
endif()

option(BUILD_EXAMPLES "Build examples" ON)
if (${BUILD_EXAMPLES})
    add_subdirectory(examples)
endif()
