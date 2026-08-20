cmake_minimum_required(VERSION 3.20)

project(nps_app LANGUAGES CXX)

# ============================================================
# NPS Output Directory
# ============================================================

set(NPS_OUTPUT_DIR
    ${CMAKE_SOURCE_DIR}/output/nps/build
)

# ============================================================
# NPS Shared Library
#
# src/test.cpp will be compiled into:
#
# Linux   -> libnps.so
# Windows -> nps.dll
# ============================================================

add_library(nps_lib SHARED
    src/test1.cpp
)

# ============================================================
# NPS Include Directory
# ============================================================

target_include_directories(nps_lib
    PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/inc
)

# ============================================================
# Shared Library Output
# ============================================================

set_target_properties(nps_lib PROPERTIES

    # libnps.so instead of libnps_lib.so
    OUTPUT_NAME nps

    # Linux .so
    LIBRARY_OUTPUT_DIRECTORY
        ${NPS_OUTPUT_DIR}

    # Windows .dll
    RUNTIME_OUTPUT_DIRECTORY
        ${NPS_OUTPUT_DIR}

    # Windows import library (.dll.a / .lib)
    ARCHIVE_OUTPUT_DIRECTORY
        ${NPS_OUTPUT_DIR}
)

# ============================================================
# NPS Executable
# ============================================================

add_executable(nps_app
    main.cpp
)

# ============================================================
# Link NPS executable with shared library
# ============================================================

target_link_libraries(nps_app
    PRIVATE
        nps_lib
)

# ============================================================
# Executable Output
# ============================================================

set_target_properties(nps_app PROPERTIES

    OUTPUT_NAME nps

    RUNTIME_OUTPUT_DIRECTORY
        ${NPS_OUTPUT_DIR}/bin
)

# ============================================================
# Clean only NPS
# ============================================================

add_custom_target(clean_nps

    COMMAND ${CMAKE_COMMAND} -E rm -rf
        ${CMAKE_SOURCE_DIR}/output/nps/build

    COMMAND ${CMAKE_COMMAND} -E rm -rf
        ${CMAKE_BINARY_DIR}/nps_app/CMakeFiles/nps_app.dir

    COMMAND ${CMAKE_COMMAND} -E rm -rf
        ${CMAKE_BINARY_DIR}/nps_app/CMakeFiles/nps_lib.dir

    COMMENT "Cleaning NPS build files"
)