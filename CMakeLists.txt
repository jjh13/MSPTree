cmake_minimum_required(VERSION 2.6)

enable_testing()
set(CMAKE_DISABLE_IN_SOURCE_BUILD ON)
set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")
ADD_DEFINITIONS( -std=c++11)

project (poisson)

MARK_AS_ADVANCED(USE_FFTWD)
OPTION(USE_FFTWF "Use single precision FFTW if found" ON)

MARK_AS_ADVANCED(LIBS)

find_package(FFTW REQUIRED)
include_directories(${FFTW_INCLUDES})
set(LIBS ${LIBS} ${FFTW_LIBRARIES})

find_package(OpenCL REQUIRED)
include_directories(${OPENCL_INCLUDE_DIRS})
set(LIBS ${LIBS} ${OPENCL_LIBRARIES})

find_package(Eigen3 REQUIRED)
include_directories(${EIGEN3_INCLUDE_DIR})

find_package(ViennaCL REQUIRED)
include_directories(${ViennaCL_INCLUDES})

find_package(OpenMP)
if(OPENMP_FOUND)
message("OpenMP Found")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()
 	 MESSAGE(STATUS ${LIBS} )

include_directories(./include)
add_executable (poisson ./src/main.cpp) 
target_link_libraries (poisson ${LIBS})

add_subdirectory(test)

