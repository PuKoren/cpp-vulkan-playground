#PROJECT NAME
project(vulkan_playground)
cmake_minimum_required(VERSION 3.7)

if(CMAKE_COMPILER_IS_GNUCC OR CMAKE_COMPILER_IS_GNUCXX)
    list(APPEND CMAKE_CXX_FLAGS "-std=c++0x ${CMAKE_CXX_FLAGS} -g -ftest-coverage -fprofile-arcs -Wall -Wno-long-long -pedantic")
endif()

#EXTERNAL PACKAGES (IRRLICHT)
#set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} "${CMAKE_SOURCE_DIR}/cmake/Modules/")

find_package(Vulkan)

if(APPLE)
    find_library(COCOA_LIBRARY Cocoa)
    find_library(IOKIT_FRAMEWORK IOKit)
    set(EXTRA_LIBS ${COCOA_LIBRARY} ${IOKIT_FRAMEWORK})
endif(APPLE)

if (NOT ${Vulkan_INCLUDE_DIRS} STREQUAL "")
    set(VULKAN_PATH ${Vulkan_INCLUDE_DIRS})
    STRING(REGEX REPLACE "/Include" "" VULKAN_PATH ${VULKAN_PATH})
endif()

#INCLUDES & SRC FILES
include_directories(
    ${PROJECT_SOURCE_DIR}/include
)

file(GLOB SRCS src/*.cpp)
file(GLOB HDRS include/*.h)

#LINKING AND EXECUTABLE
add_executable(${PROJECT_NAME} ${SRCS} ${HDRS})
target_link_libraries(${PROJECT_NAME} ${VULKAN_LIBRARIES})
