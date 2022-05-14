cmake_minimum_required(VERSION 3.8)
project(rrbot)

# Default to C11
if(NOT CMAKE_C_STANDARD)
  set(CMAKE_C_STANDARD 11)
endif()

# Default to C++14
if(NOT CMAKE_CXX_STANDARD)
  set(CMAKE_CXX_STANDARD 14)
endif()

if(NOT WIN32)
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# Common Packages
find_package(control_msgs REQUIRED)
find_package(std_msgs REQUIRED)
find_package(gazebo_plugins REQUIRED)

# C++ packages
find_package(ament_cmake REQUIRED)
find_package(rclcpp REQUIRED)
find_package(rclcpp_action REQUIRED)

# Python packages
find_package(ament_cmake_python REQUIRED)
find_package(rclpy REQUIRED)


include_directories(include)

install(DIRECTORY
  
  config
  gazebo
  launch
  meshes
  ros2_control
  rrbot
  rviz
  scripts
  worlds
  xacro
  
  DESTINATION share/${PROJECT_NAME}/
)

# C++ Executables

add_executable(cpp_node src/cpp_node.cpp)
ament_target_dependencies( cpp_node 
rclcpp
)

if(BUILD_TESTING)
  find_package(ament_cmake_gtest REQUIRED)
  find_package(ament_lint_auto REQUIRED)

  ament_lint_auto_find_test_dependencies()
endif()

# Install C++ Modules
install(TARGETS 
    
  cpp_node

  DESTINATION lib/${PROJECT_NAME}
)

# Install Python Modules
ament_python_install_package(${PROJECT_NAME})

install(PROGRAMS

  scripts/py_node.py

  DESTINATION lib/${PROJECT_NAME}
)


ament_package()
