cmake_minimum_required(VERSION 3.4)
project(boost_asio_bluetooth VERSION "0.1.0" LANGUAGES CXX)
string(REGEX MATCHALL "[0-9]" PROJECT_VERSION_PARTS "${PROJECT_VERSION}")
set(PROJECT_SOVERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}")
list(GET PROJECT_VERSION_PARTS 0 PROJECT_VERSION_MAJOR)
list(GET PROJECT_VERSION_PARTS 1 PROJECT_VERSION_MINOR)
list(GET PROJECT_VERSION_PARTS 2 PROJECT_VERSION_PATCH)
set(PROJECT_SOVERSION "${PROJECT_VERSION_MAJOR}.${PROJECT_VERSION_MINOR}")

set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

IF(NOT CMAKE_BUILD_TYPE)
   SET(CMAKE_BUILD_TYPE "Release")
   MESSAGE(STATUS ${CMAKE_BUILD_TYPE})
ENDIF()

find_package(Boost)
if (Boost_FOUND)
	include_directories($(Boost_INCLUDE_DIR))
	add_definitions("-DHAS_BOOST")
else()
	message(FATAL_ERROR "Boost not found")
endif()

SET(REQUIRED_LIBRARIES boost_system boost_thread lpthread bluetooth)

include_directories(.)
LINK_LIBRARIES(${PROJECT_NAME} ${REQUIRED_LIBRARIES})

add_executable(echo_server echoserver.cpp)
target_link_libraries(echo_server  ${REQUIRED_LIBRARIES})

add_executable(echo_client echoclient.cpp)
target_link_libraries(echo_client  ${REQUIRED_LIBRARIES})
