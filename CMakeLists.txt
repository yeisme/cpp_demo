cmake_minimum_required(VERSION 3.25)

project(cpp_demo LANGUAGES CXX)

# C++ standard configuration
set(CMAKE_C_STANDARD 17)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

set(CACHE_TOOLS scache ccache)

foreach(TOOL ${CACHE_TOOLS})
    find_program(CACHE_EXECUTABLE ${TOOL})

    if(CACHE_EXECUTABLE)
        message(STATUS "Using ${TOOL} for compiler caching.")
        set(CMAKE_C_COMPILER_LAUNCHER "${CACHE_EXECUTABLE}")
        set(CMAKE_CXX_COMPILER_LAUNCHER "${CACHE_EXECUTABLE}")
        break()
    endif()
endforeach()

if(NOT CACHE_EXECUTABLE)
    message(STATUS "No compiler caching tool found.")
endif()
