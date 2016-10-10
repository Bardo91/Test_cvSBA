#########################################
#										#
#	Online Histogram Visualizer Tool	#
#										#
#########################################

cmake_minimum_required (VERSION 2.6)
project(testcvsba)

# Collect all files
file(GLOB_RECURSE FEATUREMODELCREATOR_SOURCE_FILES_SOURCE_FILES "src/*.cpp" "src/*.h" "src/*.inl" "src/*.c")

# Compiler specific options
if(CMAKE_COMPILER_IS_GNUCXX)
    add_definitions(-std=c++11)
endif()

SET(CMAKE_MODULE_PATH "${PROJECT_SOURCE_DIR}/cmake/modules")

# Find 3rd party packages
find_package(OpenCV 3.1 REQUIRED)
set_target_properties(${OpenCV_LIBS} PROPERTIES MAP_IMPORTED_CONFIG_RELWITHDEBINFO RELEASE)
LINK_DIRECTORIES(${OpenCV_LIB_DIR})

find_package(PCL 1.8 QUIET REQUIRED)
if(${PCL_FOUND} AND ${VTK_FOUND} AND ${Boost_FOUND} AND ${FLANN_FOUND} STREQUAL "TRUE")
        MESSAGE( STATUS "Adding properly PCL and its dependencies")
        add_definitions(-DENABLE_PCL)
        add_definitions(${PCL_DEFINITIONS})
else()
        MESSAGE( FATAL_ERROR "Couldnt Add PCL and/or it's dependencies")
endif()

find_package(cvsba REQUIRED)

# Configure application
set(APP_NAME testcvsba)

add_executable(${APP_NAME} ${FEATUREMODELCREATOR_SOURCE_FILES_SOURCE_FILES})
target_include_directories(${APP_NAME} PUBLIC ${PROJECT_SOURCE_DIR}/src)

target_include_directories(${APP_NAME} PUBLIC ${OpenCV_INCLUDE_DIRS})
target_link_libraries(${APP_NAME} LINK_PUBLIC ${OpenCV_LIBS})

target_include_directories(${APP_NAME} PUBLIC ${PCL_INCLUDE_DIRS})
target_link_libraries(${APP_NAME} LINK_PUBLIC ${PCL_LIBRARIES})

link_directories(${cvsba_LIB_DIR})
target_link_libraries(${APP_NAME} LINK_PUBLIC ${cvsba_LIBS})
