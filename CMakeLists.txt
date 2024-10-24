cmake_minimum_required(VERSION 3.2)

project(torchtest)

# Set the LibTorch Directory
set(CMAKE_PREFIX_PATH /home/axian/Downloads/libtorch)

# Set the LibTorch directory
set(TORCH_DIR /home/axian/Downloads/libtorch) 

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")

# Find LibTorch
find_package(Torch REQUIRED)

# Include directories
include_directories(${TORCH_INCLUDE_DIRS})

# Download dataset
option(DOWNLOAD_MNIST "Download the MNIST dataset from the internet" ON)
if (DOWNLOAD_MNIST)
  message(STATUS "Downloading MNIST dataset")
  execute_process(
    COMMAND python ${CMAKE_CURRENT_LIST_DIR}/tools/download_mnist.py
      -d ${CMAKE_BINARY_DIR}/data
    ERROR_VARIABLE DOWNLOAD_ERROR)
  if (DOWNLOAD_ERROR)
    message(FATAL_ERROR "Error downloading MNIST dataset: ${DOWNLOAD_ERROR}")
  endif()
endif()


# Add your source files
add_executable(${PROJECT_NAME} main.cpp)

target_compile_features(torchtest PUBLIC cxx_range_for)
target_link_libraries(torchtest ${TORCH_LIBRARIES})

# Link with LibTorch libraries
target_link_libraries(${PROJECT_NAME} "${TORCH_LIBRARIES}")

set_property(TARGET ${PROJECT_NAME} PROPERTY CXX_STANDARD 17)


if (MSVC)
  file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
  add_custom_command(TARGET mnist
                     POST_BUILD
                     COMMAND ${CMAKE_COMMAND} -E copy_if_different
                     ${TORCH_DLLS}
                     $<TARGET_FILE_DIR:mnist>)
endif (MSVC)