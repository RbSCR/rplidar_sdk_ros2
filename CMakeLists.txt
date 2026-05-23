cmake_minimum_required(VERSION 3.28.3)
project(rplidar_sdk_ros2
        VERSION 2.1.0
        LANGUAGES CXX
)

if(CMAKE_COMPILER_IS_GNUCXX OR CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  add_compile_options(-Wall -Wextra -Wpedantic)
endif()

# find dependencies
find_package(ament_cmake REQUIRED)
find_package(ament_cmake_ros REQUIRED)
# uncomment the following section in order to fill in
# further dependencies manually.
# find_package(<dependency> REQUIRED)

# set SDK path
set(RPLIDAR_SDK_PATH "${PROJECT_SOURCE_DIR}/src/sdk")

set(lib_sources
    "${RPLIDAR_SDK_PATH}/src/rplidar_driver.cpp"
    "${RPLIDAR_SDK_PATH}/src/sdkcommon.h"
    "${RPLIDAR_SDK_PATH}/src/sl_async_transceiver.cpp"
    "${RPLIDAR_SDK_PATH}/src/sl_async_transceiver.h"
    "${RPLIDAR_SDK_PATH}/src/sl_crc.cpp"
    "${RPLIDAR_SDK_PATH}/src/sl_lidar_driver.cpp"
    "${RPLIDAR_SDK_PATH}/src/sl_lidarprotocol_codec.cpp"
    "${RPLIDAR_SDK_PATH}/src/sl_lidarprotocol_codec.h"
    "${RPLIDAR_SDK_PATH}/src/sl_serial_channel.cpp"
    "${RPLIDAR_SDK_PATH}/src/sl_tcp_channel.cpp"
    "${RPLIDAR_SDK_PATH}/src/sl_udp_channel.cpp"
    "${RPLIDAR_SDK_PATH}/src/arch/linux/arch_linux.h"
    "${RPLIDAR_SDK_PATH}/src/arch/linux/net_serial.cpp"
    "${RPLIDAR_SDK_PATH}/src/arch/linux/net_serial.h"
    "${RPLIDAR_SDK_PATH}/src/arch/linux/net_socket.cpp"
    "${RPLIDAR_SDK_PATH}/src/arch/linux/thread.hpp"
    "${RPLIDAR_SDK_PATH}/src/arch/linux/timer.cpp"
    "${RPLIDAR_SDK_PATH}/src/arch/linux/timer.h"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/dataunnpacker_commondef.h"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/dataunnpacker_internal.h"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/dataunpacker.cpp"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/dataunpacker.h"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/dataupacker_namespace.h"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/unpacker/handler_capsules.cpp"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/unpacker/handler_capsules.h"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/unpacker/handler_hqnode.cpp"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/unpacker/handler_hqnode.h"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/unpacker/handler_normalnode.cpp"
    "${RPLIDAR_SDK_PATH}/src/dataunpacker/unpacker/handler_normalnode.h"
    "${RPLIDAR_SDK_PATH}/src/hal/abs_rxtx.h"
    "${RPLIDAR_SDK_PATH}/src/hal/assert.h"
    "${RPLIDAR_SDK_PATH}/src/hal/byteops.h"
    "${RPLIDAR_SDK_PATH}/src/hal/byteorder.h"
    "${RPLIDAR_SDK_PATH}/src/hal/event.h"
    "${RPLIDAR_SDK_PATH}/src/hal/locker.h"
    "${RPLIDAR_SDK_PATH}/src/hal/socket.h"
    "${RPLIDAR_SDK_PATH}/src/hal/thread.cpp"
    "${RPLIDAR_SDK_PATH}/src/hal/thread.h"
    "${RPLIDAR_SDK_PATH}/src/hal/types.h"
    "${RPLIDAR_SDK_PATH}/src/hal/util.h"
    "${RPLIDAR_SDK_PATH}/src/hal/waiter.h"
)

# List of public headers and set PUBLIC_HEADER property
set(public_headers
    ${RPLIDAR_SDK_PATH}/include/rplidar_cmd.h
    ${RPLIDAR_SDK_PATH}/include/rplidar_driver.h
    ${RPLIDAR_SDK_PATH}/include/rplidar.h
    ${RPLIDAR_SDK_PATH}/include/rplidar_protocol.h
    ${RPLIDAR_SDK_PATH}/include/rptypes.h
    ${RPLIDAR_SDK_PATH}/include/sl_crc.h
    ${RPLIDAR_SDK_PATH}/include/sl_lidar_cmd.h
    ${RPLIDAR_SDK_PATH}/include/sl_lidar_driver.h
    ${RPLIDAR_SDK_PATH}/include/sl_lidar_driver_impl.h
    ${RPLIDAR_SDK_PATH}/include/sl_lidar.h
    ${RPLIDAR_SDK_PATH}/include/sl_lidar_protocol.h
    ${RPLIDAR_SDK_PATH}/include/sl_types.h
)

# add the library target
add_library(rplidar_sdk STATIC ${lib_sources})
add_library(rplidar_sdk_ros2::rplidar_sdk ALIAS rplidar_sdk)

target_compile_features(rplidar_sdk PUBLIC cxx_std_17)  # Require C++17
target_include_directories(rplidar_sdk
    PRIVATE
        $<BUILD_INTERFACE:${RPLIDAR_SDK_PATH}/src>
        $<BUILD_INTERFACE:${RPLIDAR_SDK_PATH}/include>
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/src/include>
        $<INSTALL_INTERFACE:include/${PROJECT_NAME}>
)

# set properties for the target. VERSION set the library version to the project
# version * SOVERSION set the compatibility  version for the library to the
# major number of the version
set_target_properties(rplidar_sdk
    PROPERTIES  VERSION ${PROJECT_VERSION}
                SOVERSION ${PROJECT_VERSION_MAJOR}
)

set_target_properties(rplidar_sdk PROPERTIES PUBLIC_HEADER "${public_headers}")

# Causes the visibility macros to use dllexport rather than dllimport,
# which is appropriate when building the dll but not consuming it.
target_compile_definitions(rplidar_sdk PRIVATE "RPLIDAR_SDK_ROS2_BUILDING_LIBRARY")

install(
  DIRECTORY ${RPLIDAR_SDK_PATH}/include/
  DESTINATION include/${PROJECT_NAME}
)
install(
  TARGETS rplidar_sdk
  EXPORT export_${PROJECT_NAME}
  ARCHIVE DESTINATION lib
  LIBRARY DESTINATION lib
  RUNTIME DESTINATION bin
  PUBLIC_HEADER DESTINATION include/${PROJECT_NAME}
  INCLUDES DESTINATION include/${PROJECT_NAME}
)

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

ament_export_libraries(
  rplidar_sdk
)
ament_export_targets(
  export_${PROJECT_NAME}
)

ament_package()
