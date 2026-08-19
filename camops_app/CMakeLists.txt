cmake_minimum_required(VERSION 3.20)

project(camops_app LANGUAGES CXX)

# ============================================================
# CAMOPS Output Directory
# ============================================================

set(CAMOPS_OUTPUT_DIR
    ${CMAKE_SOURCE_DIR}/output/camops/build
)


# ============================================================
# CAMOPS Shared Library
#
# src/test.cpp will be compiled into:
#
# Linux   -> libcamops.so
# Windows -> camops.dll
# ============================================================

add_library(camops_lib SHARED
    src/test.cpp
)


# ============================================================
# CAMOPS Include Directory
#
# PUBLIC means:
#
# 1. camops_lib can use headers from inc/
# 2. camops_app automatically gets access to inc/
#    because it links with camops_lib
# ============================================================

target_include_directories(camops_lib
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/inc
)


# ============================================================
# Shared Library Output
# ============================================================

set_target_properties(camops_lib PROPERTIES

    # libcamops.so instead of libcamops_lib.so
    OUTPUT_NAME camops

    # Linux .so
    LIBRARY_OUTPUT_DIRECTORY
        ${CAMOPS_OUTPUT_DIR}

    # Windows .dll
    RUNTIME_OUTPUT_DIRECTORY
        ${CAMOPS_OUTPUT_DIR}

    # Windows import library (.dll.a / .lib)
    ARCHIVE_OUTPUT_DIRECTORY
        ${CAMOPS_OUTPUT_DIR}
)


# ============================================================
# CAMOPS Executable
#
# main.cpp goes into the executable.
# src/test.cpp is NOT compiled into the executable directly.
# It is provided through camops_lib.
# ============================================================

add_executable(camops_app
    main.cpp
)


# ============================================================
# Link CAMOPS executable with shared library
# ============================================================

target_link_libraries(camops_app
    PRIVATE
        camops_lib
)


# ============================================================
# Executable Output
# ============================================================

set_target_properties(camops_app PROPERTIES

    # camops_app -> camops
    OUTPUT_NAME camops

    RUNTIME_OUTPUT_DIRECTORY
        ${CAMOPS_OUTPUT_DIR}/bin
)

add_custom_target(clean_camops
    COMMAND ${CMAKE_COMMAND} -E rm -rf
        ${CMAKE_SOURCE_DIR}/output/camops/build

    COMMAND ${CMAKE_COMMAND} -E rm -rf
        ${CMAKE_BINARY_DIR}/camops_app/CMakeFiles/camops_app.dir

    COMMAND ${CMAKE_COMMAND} -E rm -rf
        ${CMAKE_BINARY_DIR}/camops_app/CMakeFiles/camops_lib.dir

    COMMENT "Cleaning CAMOPS build files"
)