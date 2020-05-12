cmake_minimum_required(VERSION 3.8)
project(GRNN LANGUAGES CUDA CXX)

SET(CMAKE_CXX_FLAGS "${CMAKE_CXXFLAGS} -O3 -std=c++11")
SET(CMAKE_CUDA_FLAGS "${CMAKE_CXX_FLAGS} ${CMAKE_CUDA_FLAGS} -arch=compute_70 -code=sm_70 -D_FORCE_INLINES --ptxas-options='-v -warn-lmem-usage -warn-spills' --nvlink-options='-v'")

find_package(CUDA REQUIRED)
include_directories("${CUDA_INCLUDE_DIRS}")

foreach(PRGS IN ITEMS LSTM GRU_single GRU_double)
    add_executable(${PRGS} single_layer_${PRGS}.cpp
            ${PRGS}.cu ${PRGS}.h RNNBase.h misc.h)
    set_target_properties(${PRGS} PROPERTIES CUDA_SEPARABLE_COMPILATION ON)
endforeach()
