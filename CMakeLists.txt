cmake_minimum_required(VERSION 2.6 FATAL_ERROR)

project(HL_MRF)
set(CMAKE_BUILD_TYPE "Release")

# find_package (PCL 1.11
# REQUIRED COMPONENTS)

set(PCL_DIR "/home/xiaochen/pcl-1.12.0/share/pcl-1.12")
find_package(PCL 1.12 REQUIRED)

include_directories (${PCL_INCLUDE_DIRS})
link_directories (${PCL_LIBRARY_DIRS})
add_definitions (${PCL_DEFINITIONS})

find_package(Boost COMPONENTS graph filesystem)

# set(BOOST_ROOT "/usr/local/boost_1_75")
# set(BOOST_INCLUDEDIR "${BOOST_ROOT}/include")
# set(BOOST_LIBRARYDIR "${BOOST_ROOT}/lib")
# find_package(Boost 1.75.0 REQUIRED)

FIND_PACKAGE( OpenMP REQUIRED)
if(OPENMP_FOUND)
message("OPENMP FOUND")
set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS} ${OpenMP_C_FLAGS}")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${OpenMP_CXX_FLAGS}")
set(CMAKE_EXE_LINKER_FLAGS "${CMAKE_EXE_LINKER_FLAGS} ${OpenMP_EXE_LINKER_FLAGS}")
endif()


include_directories(./include)
include_directories(./include/Graph)
include_directories(./include/GROR)

set(Graph ./include/Graph/scan_graph_inference.h
                   ./include/Graph/transformation_graph.h
                   ./include/Graph/transformation_graph.hpp
                   ./src/scan_graph_inference.cpp
                   )
set(GROR ./include/GROR/include/ia_gror.h
    ./include/GROR/include/ia_gror.hpp
    ./include/GROR/include/gror_pre.h
    ./include/GROR/include/gror_common.h
    ./include/GROR/src/gror_pre.cpp
)
set(HL_MRF ./include/HL_MRF.h 
                    ./src/HL_MRF.cpp
                   )

set(SourceFile ./src/example_HL_MRF.cpp)

add_executable(HL_MRF ${Graph} ${GROR} ${HL_MRF} ${SourceFile} )
target_link_libraries(HL_MRF ${PCL_LIBRARIES} ${Boost_LIBRARIES})
