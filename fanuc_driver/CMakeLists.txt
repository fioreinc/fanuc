cmake_minimum_required(VERSION 2.8.3)

project(fanuc_driver)

find_package(catkin REQUIRED COMPONENTS industrial_robot_client)

catkin_package(
    CATKIN_DEPENDS industrial_robot_client
)

# these nodes run on the host computer, so use Linux sockets for
# simple_message sockets
add_definitions(-DLINUXSOCKETS=1)

include_directories(
    include
    ${catkin_INCLUDE_DIRS}
)


# Little Endian Fanuc specific robot state node
add_executable(fanuc_robot_state
    src/fanuc_robot_state_node.cpp
    src/fanuc_utils.cpp)
target_link_libraries(fanuc_robot_state
    industrial_robot_client
    industrial_utils)
set_target_properties(fanuc_robot_state
    PROPERTIES OUTPUT_NAME robot_state
    PREFIX "")


# Big Endian Fanuc specific robot state node
add_executable(fanuc_robot_state_bswap
    src/fanuc_robot_state_node.cpp
    src/fanuc_utils.cpp)
target_link_libraries(fanuc_robot_state_bswap
    industrial_robot_client_bswap
    industrial_utils)
set_target_properties(fanuc_robot_state_bswap
    PROPERTIES OUTPUT_NAME robot_state_bswap
    PREFIX "")


# Little Endian Fanuc specific motion streaming interface
add_executable(fanuc_motion_streaming_interface
    src/fanuc_joint_streamer_node.cpp
    src/fanuc_utils.cpp)
target_link_libraries(fanuc_motion_streaming_interface
    industrial_robot_client
    industrial_utils)
set_target_properties(fanuc_motion_streaming_interface
    PROPERTIES OUTPUT_NAME motion_streaming_interface
    PREFIX "")


# Big Endian Fanuc specific motion streaming interface
add_executable(fanuc_motion_streaming_interface_bswap
    src/fanuc_joint_streamer_node.cpp
    src/fanuc_utils.cpp)
target_link_libraries(fanuc_motion_streaming_interface_bswap
    industrial_robot_client_bswap
    industrial_utils)
set_target_properties(fanuc_motion_streaming_interface_bswap
    PROPERTIES OUTPUT_NAME motion_streaming_interface_bswap
    PREFIX "")


install(TARGETS
    fanuc_robot_state
    fanuc_robot_state_bswap

    fanuc_motion_streaming_interface
    fanuc_motion_streaming_interface_bswap

    DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

foreach(dir KAREL launch TPE)
   install(DIRECTORY ${dir}/
      DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}/${dir})
endforeach(dir)