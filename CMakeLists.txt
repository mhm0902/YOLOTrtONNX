cmake_minimum_required(VERSION 3.10)

project(yolov11)

add_definitions(-std=c++11)
add_definitions(-DAPI_EXPORTS)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_BUILD_TYPE Debug)

set(CMAKE_CUDA_COMPILER /usr/local/cuda/bin/nvcc)
enable_language(CUDA)

set(CMAKE_CUDA_ARCHITECTURES 86)

include_directories(${PROJECT_SOURCE_DIR}/include)

# include and link dirs of cuda and tensorrt, you need adapt them if yours are different
if(CMAKE_SYSTEM_PROCESSOR MATCHES "aarch64")
  message("embed_platform on")
  include_directories(/usr/local/cuda/targets/aarch64-linux/include)
  link_directories(/usr/local/cuda/targets/aarch64-linux/lib)
else()
  message("embed_platform off")

  # cuda
  include_directories(/usr/local/cuda/include)
  link_directories(/usr/local/cuda/lib64)

  # tensorrt
  include_directories(/workspace/shared/TensorRT-8.6.1.6/include)
  link_directories(/workspace/shared/TensorRT-8.6.1.6/lib)
endif()

find_package(OpenCV)
include_directories(${OpenCV_INCLUDE_DIRS})

file(GLOB_RECURSE SRCS ${PROJECT_SOURCE_DIR}/src/*.cpp ${PROJECT_SOURCE_DIR}/src/*.cu)

add_executable(yolo11_det ${SRCS})
target_link_libraries(yolo11_det nvinfer)
target_link_libraries(yolo11_det cudart)
target_link_libraries(yolo11_det ${OpenCV_LIBS})
