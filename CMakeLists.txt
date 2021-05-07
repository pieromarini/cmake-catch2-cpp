cmake_minimum_required(VERSION 3.1)

project(projname DESCRIPTION "description" LANGUAGES C CXX)

set(LIB_NAME "projlib")
set(EXEC_NAME "projexec")

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)

set(CMAKE_MODULE_PATH ${CMAKE_MODULE_PATH} ${CMAKE_CURRENT_SOURCE_DIR}/cmake)

set(CMAKE_EXPORT_COMPILE_COMMANDS ON)

add_library(project_options INTERFACE)
target_compile_features(project_options INTERFACE cxx_std_17)

# Add support for MSCV. Only gcc for now
set(COMPILE_FLAGS "-Wall -g")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${COMPILE_FLAGS}")

# Check that all submodules are present
include(cmake/SubmodulesCheck.cmake)
check_submodules()

set(LIB_DIR "${CMAKE_CURRENT_SOURCE_DIR}/lib")
set(SOURCES_DIR "${CMAKE_CURRENT_SOURCE_DIR}/src")
set(TESTS_DIR "${CMAKE_CURRENT_SOURCE_DIR}/tests")


file(GLOB_RECURSE SOURCE_FILES "${SOURCES_DIR}/*.cpp")
file(GLOB_RECURSE HEADER_FILES "${SOURCES_DIR}/*.h")
file(GLOB_RECURSE TEST_FILES "${TESTS_DIR}/*.cpp")

# [LIBRARY] Main
add_library(${LIB_NAME} ${SOURCE_FILES} ${HEADER_FILES})
target_include_directories(${LIB_NAME} PUBLIC ${SOURCES_DIR})
target_link_libraries(${LIB_NAME} project_options)

# [EXECUTABLE] Main
add_executable(${EXEC_NAME} main.cpp)
target_link_libraries(${EXEC_NAME} PUBLIC ${LIB_NAME})

# TESTING with Catch2
set(TEST_MAIN unit_tests)

add_subdirectory(lib/Catch2)
add_executable(${TEST_MAIN} ${TEST_FILES})
target_link_libraries(${TEST_MAIN} Catch2::Catch2WithMain ${LIB_NAME})

# Enables 'make tests'
add_custom_target(tests
	COMMAND "${PROJECT_BINARY_DIR}/${TEST_MAIN}"
	DEPENDS ${TEST_MAIN}
)

include(CTest)
include(Catch)
catch_discover_tests(${TEST_MAIN})
