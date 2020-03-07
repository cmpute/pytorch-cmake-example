cmake_minimum_required(VERSION 3.8 FATAL_ERROR)
project(LLTM LANGUAGES C CXX CUDA VERSION 1.0)
# if CUDA is optional: enable_language(CUDA)

find_package(Torch REQUIRED)
find_package(PythonExtensions REQUIRED)
set(CMAKE_CXX_FLAGS ${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS})

# tweak torch dependencies, see https://github.com/pytorch/pytorch/issues/33928
get_target_property(TORCH_INTERFACE_LIB torch INTERFACE_LINK_LIBRARIES)
string(REPLACE "/usr/local/cuda" ${CUDA_TOOLKIT_ROOT_DIR} TORCH_INTERFACE_LIB "${TORCH_INTERFACE_LIB}")
set_target_properties(torch PROPERTIES INTERFACE_LINK_LIBRARIES ${TORCH_INTERFACE_LIB})

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
