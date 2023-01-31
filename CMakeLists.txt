project(L0-gradient-minimization)
cmake_minimum_required(VERSION 3.14)

# .dll and .exe
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_SOURCE_DIR}/bin) 

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_STANDARD 17)

if (WIN32)
# option for Visual Studio
# -EHsc (enable proper Exxeption Handling) needs to avoid C4530
# -Wall is too noisy so that set -W4. 
# https://docs.microsoft.com/en-us/cpp/build/reference/compiler-option-warning-level?view=vs-2017
# "However, for a new project, it may be best to use /W4 in all compilations;
# this will ensure the fewest possible hard-to-find code defects."
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} /W4 /EHsc /MP /bigobj")
else()
# g++/clang option for *nix
set(CMAKE_CXX_EXTENSIONS OFF) #Set this to ON if you want to use GNU++
set(CMAKE_CXX_FLAGS  "${CMAKE_CXX_FLAGS} -Wall -O2")
endif()


##########
#Eigen
##########
if(NOT DEFINED EIGEN3_INCLUDE_DIR)
  set(EIGEN3_INCLUDE_DIR "${CMAKE_CURRENT_SOURCE_DIR}/third_party/eigen" CACHE PATH "eigen installed directory" FORCE)
endif()
message("EIGEN3_INCLUDE_DIR: ${EIGEN3_INCLUDE_DIR}") 
include_directories(${EIGEN3_INCLUDE_DIR})

##########
#cxxopts
##########
if(NOT DEFINED CXXOPTS_INSTALL_DIR)
  set(CXXOPTS_INSTALL_DIR "${CMAKE_CURRENT_SOURCE_DIR}/third_party/cxxopts" CACHE PATH "cxxopts installed directory" FORCE)
endif()
message("CXXOPTS_INSTALL_DIR: ${CXXOPTS_INSTALL_DIR}") 
include_directories(${CXXOPTS_INSTALL_DIR}/include)

##########################
# opencv
##########################
find_package(OpenCV REQUIRED)
if(OpenCV_FOUND)
  message(STATUS "Found 'opencv library' ${OpenCV_VERSION}")
else()
  message(SEND_ERROR "Couldn't find OpenCV")
endif()

add_executable(${PROJECT_NAME} L0minimization.cpp)
target_link_libraries(${PROJECT_NAME} ${OpenCV_LIBS})