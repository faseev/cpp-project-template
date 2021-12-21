cmake_minimum_required (VERSION 3.18)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED True)
set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(PROJECT_INSTALL_DIR ${CMAKE_SOURCE_DIR}/dist)

#add_compile_options(/EHsc /Wall)

if ($ENV{CI_PIPELINE_ID})
	project(simpleproject VERSION 0.0.$ENV{CI_PIPELINE_ID})
else()
	project(simpleproject VERSION 0.0.1)
endif()

set(PROJECT_SOURCE_DIR ${PROJECT_SOURCE_DIR}/src)
add_subdirectory(src)

option(BUILD_TESTS "Build tests" $ENV{BUILD_TESTS})
if (BUILD_TESTS)
    add_compile_definitions(BUILD_TESTS)

    enable_testing()
    
    add_subdirectory(${CMAKE_SOURCE_DIR}/libs/googletest)
    include_directories(${CMAKE_SOURCE_DIR}/libs/googletest/googletest/include)

    set(gtest_force_shared_crt ON CACHE BOOL "" FORCE)

    add_subdirectory(tests)
else()
    message("For building tests use -DBBUILD_TESTS=Yes option.")
endif()
