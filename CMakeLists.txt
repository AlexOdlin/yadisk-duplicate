cmake_minimum_required(VERSION 3.0)
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

include("gate/cmake/HunterGate.cmake")
HunterGate(
    URL  "https://github.com/designerror/hunter/archive/v0.18.4.20.tar.gz"
    SHA1 "94e3314bfe952db1510ca644d2a8e0b55e9ebc8b"
)

project(duplicate)

set(DUPLICATE_MAJOR 0)
set(DUPLICATE_VERSION_MINOR 1)
set(DUPLICATE_VERSION_PATCH 1)
set(DUPLICATE_VERSION ${DUPLICATE_VERSION_MAJOR}.${DUPLICATE_VERSION_MINOR}.${DUPLICATE_VERSION_PATCH})
set(DUPLICATE_VERSION_STRING "v${DUPLICATE_VERSION}")
SET(PROJECT_NAME "duplicate") 

option(BUILD_TESTS "Build tests" ON)

include_directories(${CMAKE_CURRENT_SOURCE_DIR}/include)

file(GLOB ${PROJECT_NAME}_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/sources/*) 

if(BUILD_TESTS)
	enable_testing()
	hunter_add_package(Catch)
	find_package(Catch CONFIG REQUIRED)
	file(GLOB TESTS_${PROJECT_NAME}_SOURCES ${CMAKE_CURRENT_SOURCE_DIR}/tests/*/*.cpp)
	add_executable(check ${TESTS_${PROJECT_NAME}_SOURCES} ${CMAKE_CURRENT_SOURCE_DIR}/tests/main.cpp)
	target_link_libraries(check ${PROJECT_NAME} Catch::Catch)
	add_test(NAME check COMMAND check "-s" "-r" "compact" "--use-colour" "yes")	
endif()
