cmake_minimum_required(VERSION 3.8)
project(sick_visionary_ros2)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

if(BUILD_TESTING)
  find_package(ament_lint_auto REQUIRED)
  # the following line skips the linter which checks for copyrights
  # comment the line when a copyright and license is added to all source files
  set(ament_cmake_copyright_FOUND TRUE)
  # the following line skips cpplint (only works in a git repo)
  # comment the line when this package is in a git repo and when
  # a copyright and license is added to all source files
  set(ament_cmake_cpplint_FOUND TRUE)
  ament_lint_auto_find_test_dependencies()
endif()
cmake_minimum_required(VERSION 3.8)
find_package(cv_bridge REQUIRED)
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(sensor_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(diagnostic_updater REQUIRED) # https://github.com/ros/diagnostics/tree/ros2-humble/diagnostic_updater also can get with apt
find_package(image_transport REQUIRED)


# find_package(catkin REQUIRED COMPONENTS TODO: convert this to package.xml
#   pcl_conversions
#   pcl_ros
# )

find_package(Boost REQUIRED COMPONENTS system)
find_package(OpenCV REQUIRED)
find_package(PCL 1.3 REQUIRED)

### COMPILER FLAGS ###
if(NOT CMAKE_BUILD_TYPE)
  set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)
set(CMAKE_CXX_FLAGS "-Wall -Wextra")
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

### BUILD ###
include_directories(
  ${catkin_INCLUDE_DIRS}
  ${PCL_INCLUDE_DIRS}
  include
  sick_visionary_cpp_shared
)

add_subdirectory(sick_visionary_cpp_shared)

add_executable(sick_visionary_t_mini_node src/visionary_t_mini.cpp)
add_executable(sick_visionary_s_node src/visionary_s.cpp)

target_link_libraries(sick_visionary_t_mini_node
  sick_visionary_cpp_shared
  ${catkin_LIBRARIES}
  ${OpenCV_LIBS}
  ${PCL_LIBRARY}
)

target_link_libraries(sick_visionary_s_node
  sick_visionary_cpp_shared
  ${catkin_LIBRARIES}
  ${OpenCV_LIBS}
  ${PCL_LIBRARY}
)

add_definitions(${PCL_DEFINITIONS})

### TEST ###


### INSTALL ###
install(TARGETS sick_visionary_t_mini_node sick_visionary_s_node
  ARCHIVE DESTINATION lib/${PROJECT_NAME} 
)

install(DIRECTORY launch
  DESTINATION share/${PROJECT_NAME}/
)


ament_package()
