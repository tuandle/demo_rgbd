cmake_minimum_required(VERSION 2.8.3)
project(demo_rgbd)

set(CMAKE_BUILD_TYPE Release)

find_package(catkin REQUIRED COMPONENTS
  roscpp
  sensor_msgs
  std_msgs
  cv_bridge
  pcl_ros
)

find_package(Eigen3 REQUIRED QUIET)
find_package(PCL REQUIRED QUIET)
set(OpenCV_DIR $ENV{OPENCV2_INSTALL_DIR})
find_package(OpenCV 2 REQUIRED QUIET)

include_directories(
  ${catkin_INCLUDE_DIRS}
  ${OpenCV_INCLUDE_DIRS}
  ${EIGEN3_INCLUDE_DIR}
  ${PCL_INCLUDE_DIRS}
)

LIST(APPEND LINK_LIBS
  ${catkin_LIBRARIES}
  ${PCL_LIBRARIES}
  ${OpenCV_LIBRARIES}
)

catkin_package(
  CATKIN_DEPENDS roscpp std_msgs sensor_msgs cv_bridge pcl_ros
  DEPENDS EIGEN3 PCL OpenCV
)

SET(CMAKE_MODULE_PATH ${PROJECT_SOURCE_DIR}/cmake)
include_directories("${PROJECT_SOURCE_DIR}/include")

add_subdirectory(isamlib)
link_libraries(isamlib)

add_executable(featureTracking_rgbd src/featureTracking.cpp)
add_executable(visualOdometry_rgbd src/visualOdometry.cpp)
add_executable(bundleAdjust_rgbd src/bundleAdjust.cpp)
add_executable(processDepthmap_rgbd src/processDepthmap.cpp)
add_executable(stackDepthPoint_rgbd src/stackDepthPoint.cpp)
add_executable(transformMaintenance_rgbd src/transformMaintenance.cpp)
add_executable(registerPointCloud_rgbd src/registerPointCloud.cpp)


target_link_libraries(featureTracking_rgbd ${LINK_LIBS})
target_link_libraries(visualOdometry_rgbd  ${LINK_LIBS})
target_link_libraries(bundleAdjust_rgbd  ${LINK_LIBS})
target_link_libraries(processDepthmap_rgbd  ${LINK_LIBS})
target_link_libraries(stackDepthPoint_rgbd  ${LINK_LIBS})
target_link_libraries(transformMaintenance_rgbd  ${LINK_LIBS})
target_link_libraries(registerPointCloud_rgbd  ${LINK_LIBS})
