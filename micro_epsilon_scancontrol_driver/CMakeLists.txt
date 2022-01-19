cmake_minimum_required(VERSION 3.1.3)
project(micro_epsilon_scancontrol_driver)

find_package(catkin REQUIRED COMPONENTS roscpp rospy rqt_gui rqt_gui_py pcl_ros nodelet micro_epsilon_scancontrol_msgs)
find_package(PkgConfig REQUIRED)

pkg_check_modules(scancontrol REQUIRED glib-2.0 aravis-0.6 mescan llt)

catkin_python_setup()

catkin_package(
  INCLUDE_DIRS include
  CATKIN_DEPENDS roscpp rospy rqt_gui rqt_gui_py pcl_ros nodelet micro_epsilon_scancontrol_msgs
  DEPENDS scancontrol
)

include_directories(include ${catkin_INCLUDE_DIRS} ${scancontrol_INCLUDE_DIRS})

add_executable(driver_node src/node.cpp src/driver.cpp)
target_link_libraries(driver_node ${catkin_LIBRARIES} ${scancontrol_LIBRARIES})
#add_dependencies(driver_node)

add_library(driver_nodelet src/nodelet.cpp src/driver.cpp)
target_link_libraries(driver_nodelet ${catkin_LIBRARIES} ${scancontrol_LIBRARIES})
#add_dependencies(driver_nodelet)

install(
  TARGETS
    driver_node
  RUNTIME DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)

install(
  TARGETS
  driver_nodelet
  ARCHIVE DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  LIBRARY DESTINATION ${CATKIN_PACKAGE_LIB_DESTINATION}
  RUNTIME DESTINATION ${CATKIN_GLOBAL_BIN_DESTINATION}
)

# Mark cpp header files for installation
install(DIRECTORY include/${PROJECT_NAME}/ DESTINATION ${CATKIN_PACKAGE_INCLUDE_DESTINATION})

# Mark other files for installation (e.g. launch and bag files, etc.)
install(DIRECTORY config DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY launch DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(DIRECTORY resource DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(FILES me_scancontrol_nodelet.xml DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
install(FILES me_scancontrol_gui.xml DESTINATION ${CATKIN_PACKAGE_SHARE_DESTINATION})
