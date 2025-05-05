cmake_minimum_required(VERSION 3.30)

# C++ standard configuration
set(CMAKE_C_STANDARD 17)
set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

# 编译缓存配置
if(MSVC)
    # MSVC 编译器 - 使用内置的缓存和热重载功能
    message(STATUS "使用 MSVC 编译器，启用内置缓存功能")

    # 设置 PDB 文件路径，避免并行构建时的冲突
    set(CMAKE_PDB_OUTPUT_DIRECTORY "${CMAKE_BINARY_DIR}/pdb")
else()
    # 非 MSVC 编译器 - 尝试使用 sccache 或 ccache 加速编译
    find_program(SCCACHE sccache)
    find_program(CCACHE ccache)

    if(NOT SCCACHE STREQUAL "SCCACHE-NOTFOUND")
        message(STATUS "sccache found, configuring sccache.")
        set(CMAKE_C_COMPILER_LAUNCHER ${SCCACHE})
        set(CMAKE_CXX_COMPILER_LAUNCHER ${SCCACHE})
    elseif(NOT CCACHE STREQUAL "CCACHE-NOTFOUND")
        message(STATUS "ccache found, configuring ccache.")
        set(CMAKE_C_COMPILER_LAUNCHER ${CCACHE})
        set(CMAKE_CXX_COMPILER_LAUNCHER ${CCACHE})
    else()
        message(STATUS "sccache and ccache not found, skipping cache configuration.")
    endif()
endif()
