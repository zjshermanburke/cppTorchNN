cmake_minimum_required(VERSION 3.2)

project(CPPTORCHNN)

# Set the LibTorch Directory
set(CMAKE_PREFIX_PATH C:/Users/zjshe/Downloads/libtorch)

# Set the LibTorch directory
# set(LIBTORCH_DIR C:/Users/zjshe/Downloads/libtorch) 

# Find LibTorch
find_package(Torch REQUIRED)
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

set_target_properties(${PROJECT_NAME}
        PROPERTIES CUDA_SEPARABLE_COMPILATION ON)

# Include directories
include_directories(${TORCH_INCLUDE_DIRS})

# Add your source files
add_executable(${PROJECT_NAME} cppTorchNN.cpp)

# Link with LibTorch libraries
target_link_libraries(${PROJECT_NAME} "${TORCH_LIBRARIES}")

set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 17)
