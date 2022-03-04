cmake_minimum_required(VERSION 2.8)
project(ORB_SLAM2_ROS)

set(CMAKE_C_FLAGS "${CMAKE_C_FLAGS}  -Wall  -O3 -march=native ")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -Wall  -O3 -march=native")

add_definitions("-std=c++11")

find_package(OpenCV REQUIRED)
find_package(Eigen3 REQUIRED)
find_package(Pangolin REQUIRED)
find_package(PCL 1.7 REQUIRED)

find_package(catkin REQUIRED COMPONENTS
    ORB_SLAM2_LIB
    roscpp
    tf
    std_msgs
    sensor_msgs
    image_transport
    image_geometry
    cv_bridge
    pcl_ros
    pcl_conversions
    message_generation
    octomap_ros
)

add_message_files(
    DIRECTORY msg
    FILES
    PoseArrayOfArrays.msg
)

generate_messages(
    DEPENDENCIES
    std_msgs
    geometry_msgs
)

catkin_package(CATKIN_DEPENDS message_runtime std_msgs geometry_msgs)


include_directories(
    ${roscpp_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS}
    ${catkin_INCLUDE_DIRS}
    ${Pangolin_INCLUDE_DIRS}
    ${PROJECT_SOURCE_DIR}
    ${PROJECT_SOURCE_DIR}/../ORB_SLAM2_LIB
    ${PROJECT_SOURCE_DIR}/../ORB_SLAM2_LIB/include
)


set(LIBS 
   ${OpenCV_LIBS} 
   ${roscpp_LIBRARIES}
   ${catkin_LIBRARIES}
)

# Node for monocular camera
# add_executable(Mono
# src/ros_mono.cc
# )

# target_link_libraries(Mono
# ${LIBS}
# )

add_executable(Monopub
src/ros_mono_pub.cc
)

target_link_libraries(Monopub
${LIBS}
)

add_executable(Monosub
src/ros_mono_sub.cc
)

target_link_libraries(Monosub
${LIBS}
)

# Node for monocular camera (Augmented Reality Demo)
# add_executable(MonoAR
# src/AR/ros_mono_ar.cc
# src/AR/ViewerAR.h
# src/AR/ViewerAR.cc
# )

# target_link_libraries(MonoAR
# ${LIBS}
# )

# Node for stereo camera
# add_executable(Stereo
# src/ros_stereo.cc
# )

# target_link_libraries(Stereo
# ${LIBS}
# )

# # Node for RGB-D camera
# add_executable(RGBD
# src/ros_rgbd.cc
# )

# target_link_libraries(RGBD
# ${LIBS}
# )

