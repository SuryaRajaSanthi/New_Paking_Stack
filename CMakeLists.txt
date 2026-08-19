cmake_minimum_required(VERSION 3.20)

project(NEW_PAKING_STACK LANGUAGES CXX)


# ============================================================
# C++ Standard
# ============================================================

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)


# ============================================================
# Applications
# ============================================================

add_subdirectory(camops_app)

# Enable this after we configure NPS
# add_subdirectory(nps_app)