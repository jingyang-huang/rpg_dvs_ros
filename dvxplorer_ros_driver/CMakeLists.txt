cmake_minimum_required(VERSION 3.1.3)
project(dvxplorer_ros_driver)

find_package(catkin_simple REQUIRED)
find_package(libcaer REQUIRED)

catkin_simple()

SET(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -O3")
find_package(OpenCV REQUIRED)

# Setup Default Build Type as Release
if (NOT CMAKE_BUILD_TYPE)
    set(CMAKE_BUILD_TYPE "Release" CACHE STRING "" FORCE)
endif ()

find_package(Boost REQUIRED COMPONENTS system thread)

# make the executable
cs_add_executable(dvxplorer_ros_driver
  src/driver_node.cpp
  src/driver.cpp
)

# make the nodelet into a library
cs_add_library(dvxplorer_ros_driver_nodelet
  src/driver_nodelet.cpp
  src/driver.cpp
)

# link the executable to the necessary libs
target_link_libraries(dvxplorer_ros_driver
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES}
  ${OpenCV_LIBRARIES}
  libcaer::caer
)

# link the executable to the necessary libs
target_link_libraries(dvxplorer_ros_driver_nodelet
  ${catkin_LIBRARIES}
  ${Boost_LIBRARIES}
  ${OpenCV_LIBRARIES}
  libcaer::caer
)

# Install the nodelet library
cs_install()

# Install other support files for installation
install(FILES dvxplorer_ros_driver_nodelet.xml
  DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION}
)

# Export package
cs_export()
