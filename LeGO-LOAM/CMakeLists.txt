cmake_minimum_required(VERSION 2.8.3)
project(lego_loam_bor)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3 -g ")

#

find_package(catkin REQUIRED COMPONENTS
  tf
  roscpp
  rospy
  image_transport
  rosbag_storage

  pcl_ros
  pcl_conversions

  std_msgs
  sensor_msgs
  geometry_msgs
  nav_msgs
  cloud_msgs
)

find_package(GTSAM REQUIRED QUIET)
find_package(PCL REQUIRED QUIET)

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS cloud_msgs rosbag_storage
  DEPENDS PCL
)

include_directories(
	include
	${catkin_INCLUDE_DIRS}
	${PCL_INCLUDE_DIRS}
	${GTSAM_INCLUDE_DIR}
  /home/javier/Documents/UPMResearchCAR-Robocity/pyWrapper/dependencies/optimization
	/home/javier/Documents/UPMResearchCAR-Robocity/cloudMatcher/projects/LeGO-LOAM-BOR/LeGO-LOAM/src
	/home/javier/Documents/UPMResearchCAR-Robocity/cloudMatcher
)

link_directories(
	include
	${PCL_LIBRARY_DIRS}
	${GTSAM_LIBRARY_DIRS}
  /home/javier/Documents/UPMResearchCAR-Robocity/pyWrapper/dependencies/libs

)

add_executable(lego_loam_bor
    src/imageProjection.cpp
    src/featureAssociation.cpp
    src/mapOptmization.cpp
    src/transformFusion.cpp
    src/main.cpp)


add_dependencies(lego_loam_bor ${catkin_EXPORTED_TARGETS} cloud_msgs_gencpp)
target_link_libraries(lego_loam_bor ${catkin_LIBRARIES} ${PCL_LIBRARIES} gtsam graph utils bitscan copt)

