
# cuda
find_package(CUDA REQUIRED)
set(CUDA_NVCC_FLAGS "${CUDA_NVCC_FLAGS} -std=c++11 -O3")
list(APPEND renderer_inc ${CUDA_INCLUDE_DIRS})
list(APPEND renderer_lib ${CUDA_LIBRARIES})


# assimp, load ply
find_package(ASSIMP REQUIRED)
list(APPEND renderer_inc ${ASSIMP_INCLUDE_DIRS})
list(APPEND renderer_lib ${ASSIMP_LIBRARIES})


# opencv
set(CUDA_TOOLKIT_ROOT_DIR /usr/local/cuda-8.0)
set(CMAKE_PREFIX_PATH ${CMAKE_PREFIX_PATH} /home/meiqua/anaconda3/envs/furnace)
find_package(OpenCV 3 REQUIRED)
list(APPEND renderer_inc ${OpenCV_INCLUDE_DIRS})
list(APPEND renderer_lib ${OpenCV_LIBS})


# src
SET(renderer_cuda renderer.cu)
SET(renderer_srcs renderer.cpp)
CUDA_COMPILE(renderer_cuda_objs ${renderer_cuda})


# lib & test exe
add_library(cuda_renderer
               ${renderer_srcs}
               ${renderer_cuda} 
               ${renderer_cuda_objs} 
)
target_include_directories(cuda_renderer PRIVATE ${renderer_inc})
target_link_libraries(cuda_renderer PRIVATE ${renderer_lib})


add_executable(renderer_test test.cpp)
target_link_libraries(renderer_test cuda_renderer)