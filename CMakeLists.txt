cmake_minimum_required(VERSION 3.8 FATAL_ERROR)
project(LLTM LANGUAGES C CXX CUDA VERSION 1.0)
# if CUDA is optional: enable_language(CUDA)

find_package(Torch REQUIRED)
find_package(PythonExtensions REQUIRED)
set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS})

# tweak torch compiling flags, see https://github.com/pytorch/pytorch/pull/36021
set_property(TARGET torch_cuda PROPERTY INTERFACE_COMPILE_OPTIONS "")  
set_property(TARGET torch_cpu PROPERTY INTERFACE_COMPILE_OPTIONS "")

# add library
add_library(lltm_ext MODULE
    lltm/lltm.cpp
    lltm/lltm_cuda.cu
)
python_extension_module(lltm_ext)
target_link_libraries(lltm_ext ${TORCH_LIBRARIES})
target_include_directories(lltm_ext PRIVATE ${TORCH_INCLUDE_DIRS})
set_property(TARGET lltm_ext PROPERTY CXX_STANDARD 14)

install(TARGETS lltm_ext DESTINATION lltm)
