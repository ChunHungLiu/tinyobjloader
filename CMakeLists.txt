#Tiny Object Loader Cmake configuration file.
#This configures the Cmake system with multiple properties, depending
#on the platform and configuration it is set to build in.
project(tinyobjloader)
cmake_minimum_required(VERSION 2.8.6)

#Folder Shortcuts
set(TINYOBJLOADEREXAMPLES_DIR ${CMAKE_CURRENT_SOURCE_DIR}/examples)

set(tinyobjloader-Source
	${CMAKE_CURRENT_SOURCE_DIR}/tiny_obj_loader.h
	${CMAKE_CURRENT_SOURCE_DIR}/tiny_obj_loader.cc
	)

set(tinyobjloader-Example-Source
	${CMAKE_CURRENT_SOURCE_DIR}/loader_example.cc
	)

set(tinyobjloader-examples-objsticher
	${TINYOBJLOADEREXAMPLES_DIR}/obj_sticher/obj_writer.h
	${TINYOBJLOADEREXAMPLES_DIR}/obj_sticher/obj_writer.cc
	${TINYOBJLOADEREXAMPLES_DIR}/obj_sticher/obj_sticher.cc
	)

option(TINYOBJLOADER_BUILD_TEST_LOADER "Build Example Loader Application" OFF)
option(TINYOBJLOADER_COMPILATION_SHARED "Build as shared library" OFF)

if (TINYOBJLOADER_COMPILATION_SHARED)
	add_library(tinyobjloader SHARED ${tinyobjloader-Source})
else()
	add_library(tinyobjloader STATIC ${tinyobjloader-Source})
endif()

if(TINYOBJLOADER_BUILD_TEST_LOADER)
  add_executable(test_loader ${tinyobjloader-Example-Source})
  target_link_libraries(test_loader tinyobjloader)
endif()

option(TINYOBJLOADER_BUILD_OBJ_STICHER "Build OBJ Sticher Application" OFF)
if (TINYOBJLOADER_BUILD_OBJ_STICHER)
  add_executable(obj_sticher ${tinyobjloader-examples-objsticher})
  target_link_libraries(obj_sticher tinyobjloader)

  install ( TARGETS
    obj_sticher
    DESTINATION
    bin
    )
endif()

#Installation
install ( TARGETS
  tinyobjloader
  DESTINATION
  lib
  )
install ( FILES
  tiny_obj_loader.h
  DESTINATION
  include
  )
