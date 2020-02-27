cmake_minimum_required(VERSION 3.5 FATAL_ERROR)
project(LLTM LANGUAGES CXX CUDA VERSION 1.0)

find_package(Torch REQUIRED)
find_package(pybind11 REQUIRED)

pybind11_add_module(lltm_ext
    src/lltm.cpp
    src/lltm_cuda.cpp
)

# Required for pybind11 when building STATIC libraries.
# It's ON by default with SHARED or MODULE libraries.
set_property(TARGET lltm_ext PROPERTY POSITION_INDEPENDENT_CODE ON)

install(TARGETS lltm_ext DESTINATION lltm)
install(FILES lltm/__init__.py DESTINATION lltm)
