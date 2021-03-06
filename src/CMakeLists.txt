cmake_minimum_required(VERSION 3.1)

project(segmentation_sgm)

include_directories(../include)

# OpenCV
find_package(OpenCV REQUIRED)
include_directories(${OpenCV_INCLUDE_DIRS})

# CUDA
find_package(CUDA REQUIRED)
set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} ${CUDA_ARCH} -lineinfo")

# Flags for GNUCXX
if (CMAKE_COMPILER_IS_GNUCXX)
	set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -O3 -Wall")
	set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} -std=c++11")
endif()

file(GLOB srcs *.cpp *.h* *.cu ../include/segmentation_sgm.h)
cuda_add_library(segmentation_sgm STATIC ${srcs})
target_link_libraries(segmentation_sgm ${OpenCV_LIBS})
