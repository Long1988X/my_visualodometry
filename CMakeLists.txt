cmake_minimum_required( VERSION 3.0 )

project( MY_VO )

set( CMAKE_CXX_FLAGS "-std=c++11 -O2" )
set( CMAKE_BUILD_TYPE Release )

list( APPEND CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake_modules )
set( LIBRARY_OUTPUT_PATH ${PROJECT_SOURCE_DIR}/lib )

################# dependencies ##################
# Eigen
include_directories( /usr/include/eigen3 )

# Sophus
find_package( Sophus REQUIRED )
include_directories( ${Sophus_INCLUDE_DIRS} )

# OpenCV
find_package( OpenCV REQUIRED )
include_directories( ${OpenCV_INCLUDE_DIRS} )

# csparse
find_package( CSparse REQUIRED )
include_directories( ${CSPARSE_INCLUDE_DIR} )

# G2O
find_package( G2O REQUIRED )
include_directories( ${G2O_INCLUDE_DIRS} )

# Pangolin
find_package( Pangolin REQUIRED )
include_directories( ${Pangolin_INCLUDE_DIRS} )

# Glog
find_package( Glog REQUIRED )
include_directories( ${Glog_INCLUDE_DIRS} )

# gtest
find_package( GTest REQUIRED )
include_directories( ${GTEST_INCLUDE_DIRS} )

# gflags
find_package( GFlags REQUIRED )
include_directories( ${GFLAGS_INCLUDE_DIRS} )

set (THIRD_PARTY_LIBS
    ${Sophus_LIBRARIES}
    ${OpenCV_LIBS}
    ${CSPARSE_LIBRARY}
    g2o_core g2o_stuff g2o_types_sba g2o_solver_csparse
    ${Pangolin_LIBRARIES}
    ${GTEST_BOTH_LIBRARIES}
    ${GLOG_LIBRARIES}
    ${GFLAGS_LIBRARIES}
    )

include_directories( ${PROJECT_SOURCE_DIR}/include )

add_subdirectory( src )

add_executable( ok run_kitti_stereo.cpp )

target_link_libraries( ok Sophus::Sophus )
target_link_libraries( ok ${THIRD_PARTY_LIBS} myvo )