# The version of CMake we're targeting
cmake_minimum_required (VERSION 2.8)

# The name of this project.
project (eye)

# OpenCV
find_package(OpenCV REQUIRED)
include_directories(OpenCV_INCLUDE_DIRS)

# FFMPEG
find_path(AVCODEC_INCLUDE_DIR
              PREFIXES libavcodec
              NAMES avcodec.h)
include_directories(${AVCODEC_INCLUDE_DIR})

find_path(AVFORMAT_INCLUDE_DIR
              PREFIXES libavformat
              NAMES avformat.h)
include_directories(${AVFORMAT_INCLUDE_DIR})

find_path(SWSCALE_INCLUDE_DIR
                  PREFIXES libswscale
                  NAMES swscale.h)
include_directories(${SWSCALE_INCLUDE_DIR})

find_library(AVCODEC_LIB NAMES avcodec)
find_library(AVFORMAT_LIB NAMES avformat)
find_library(AVUTIL_LIB NAMES avutil)
find_library(SWSCALE_LIB NAMES swscale)

set (FFMPEG_LIBS
                ${AVCODEC_LIB}
                ${AVFORMAT_LIB}
                ${AVUTIL_LIB}
                ${SWSCALE_LIB})

# Video for Linux
find_path(V4L_INCLUDE_DIR NAMES libv4l2.h)
include_directories(V4L_INCLUDE_DIR)

find_library(V4L_LIB NAMES v4l2)

# Our sources
get_filename_component (my_folder . ABSOLUTE)
include_directories (${my_folder})

set (camera_sources
        ${my_folder}/camera.cpp
        ${my_folder}/OCVCapture.cpp)
        
# Specify an executable that includes all of the above
# sources and link libraries

add_executable(camera
        ${camera_sources})

target_link_libraries(camera
        ${OpenCV_LIBS})