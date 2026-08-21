cmake_minimum_required(VERSION 3.10.0)

project(RobotDriver VERSION 0.1.0 LANGUAGES C CXX)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS OFF)

if(MSVC)
	add_compile_options("$<$<C_COMPILE_ID:MSVC>:/utf-8>")
	add_compile_options("$<$<CXX_COMPILE_ID:MSVC>:/utf-8>")
endif()

set(CMAKE_PREFIX_PATH "D:\cpplib" $(CMAKE_PREFIX_PATH))
if(NOT TARGET spdlog)
	find_package(spdlog REQUIRED)
endif()


find_package(fmt REQUIRED)

find_package(Eigen3 3.4.1 REQUIRED NO_MODULE)

set(INCLUDE_DIR ${CMAKE_SOURCE_DIR}/include)
set(SDK_INCLUDE_DIR ${CMAKE_SOURCE_DIR}/include/crobotsdk)

file(GLOB_RECURSE SOURCES
	${CMAKE_SOURCE_DIR}/src/*.cpp
) 

add_executable(RobotDriver ${SOURCES})
	include/driver_interfaces/robot_fsm.hpp
	include/robot_logger/logger_interface.hpp)

target_link_libraries(RobotDriver PRIVATE Eigen3::Eigen)
target_link_libraries(RobotDriver PRIVATE spdlog::spdlog $<$<BOOL:${MINGW}>:ws2_32>)
target_link_libraries(RobotDriver PRIVATE fmt::fmt)

target_include_directories(RobotDriver PRIVATE
	${INCLUDE_DIR}
	${SDK_INCLUDE_DIR}
	${INCLUDE_DIR}/robot_logger
)

> Written with [StackEdit](https://stackedit.io/).
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTgyMTczMTk2Nl19
-->