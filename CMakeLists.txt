cmake_minimum_required(VERSION 2.8.3)
project(demo)
add_compile_options(-std=c++11)

find_package(Boost REQUIRED COMPONENTS date_time thread)
find_package(catkin REQUIRED COMPONENTS message_generation rostime roscpp rosconsole roscpp_serialization
  sensor_msgs
  image_transport
  cv_bridge)

find_package(OpenCV 3.3.1 REQUIRED)
find_package(Boost REQUIRED COMPONENTS thread)

include_directories(${catkin_INCLUDE_DIRS})
link_directories(${catkin_LIBRARY_DIRS})

# add_service_files(DIRECTORY srv FILES TwoInts.srv)
# generate_messages(DEPENDENCIES std_msgs)

catkin_package(CATKIN_DEPENDS message_runtime std_msgs)

macro(rosflyvision T)
  FILE(GLOB_RECURSE sourcefiles "${T}/*.cpp")
  add_executable(${T} ${sourcefiles})
  target_link_libraries(${T} ${catkin_LIBRARIES} ${Boost_LIBRARIES} ${OpenCV_LIBS} ${Boost_LIBRARIES})
  add_dependencies(${T} roscpp_tutorials_gencpp)
  install(TARGETS ${T}
    RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION})
endmacro()

foreach(dir
    web_cam
    kcf_tracker
    add_memery
    GroundStation
    )
  rosflyvision(${dir})
endforeach()


target_link_libraries(add_memery
   ${catkin_LIBRARIES}  -lrt -DBUILD_TESTS=OFF
 )

target_link_libraries(kcf_tracker
   ${catkin_LIBRARIES}  -lrt -DBUILD_TESTS=OFF
 )

