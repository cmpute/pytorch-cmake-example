# PyTorch CMake Example

This repository is an example for creating CMake-based pytorch CUDA extension. It's modified from [pytorch extension example](https://github.com/pytorch/extension-cpp) and [scikit-build example](https://github.com/scikit-build/scikit-build-sample-projects).

To build this repository, install essential requirements and then execute `python setup.py build`. If you want to use CUDA in custom location (for example your library is installed from `conda install cudatoolkit-dev -c conda-forge`), you can give hint to CMake by defining CMake definition `CMAKE_CUDA_COMPILER`.

Note: This version only works for pytorch after commit https://github.com/pytorch/pytorch/commit/e33c3e49d5549831c14b292b1383a3d192f826ab
