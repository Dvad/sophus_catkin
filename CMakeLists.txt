cmake_minimum_required(VERSION 3.0)
project(sophus_catkin)

find_package(catkin_simple REQUIRED)
catkin_simple()

include(ExternalProject)

set(SOPHUS_TAG 00f3fd91c153ef04432ae26450080fd3357f421d)

file(MAKE_DIRECTORY ${CATKIN_DEVEL_PREFIX}/include)

ExternalProject_Add(sophus_src
  GIT_REPOSITORY  https://github.com/strasdat/Sophus
  GIT_TAG ${SOPHUS_TAG}
  CMAKE_ARGS -DCMAKE_INSTALL_PREFIX:PATH=${CATKIN_DEVEL_PREFIX} -DBUILD_SHARED_LIBS=ON
)

cs_add_library(${PROJECT_NAME} src/dependency_tracker.cc)
add_dependencies(${PROJECT_NAME} sophus_src)

cs_install()
install(DIRECTORY ${CATKIN_DEVEL_PREFIX}/include
		DESTINATION ${CATKIN_GLOBAL_INCLUDE_DESTINATION}
		FILES_MATCHING PATTERN "*.hpp"
		PATTERN "*.h"
		PATTERN ".svn" EXCLUDE
)

cs_export(INCLUDE_DIRS ${CATKIN_DEVEL_PREFIX}/include)