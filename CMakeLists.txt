# Request the minimum version of CMake, in case of lower version throws error.
cmake_minimum_required(VERSION 3.25 FATAL_ERROR)

# Define your project.
project(
    "FirstProjectInCMake"
    VERSION 1.0.0
    DESCRIPTION "CMake C Project Template for Operating Systems II"
    LANGUAGES C
)

# Enable testing and add the test
include(CTest)

# Define the C statndard, we are going to use std17
set(CMAKE_C_STARDARD 17)

# Include the 'include' directory, where the header are located 
include_directories(include)

# To automatic download external deps, we need use 'FectchContent'
include (FetchContent)

# In this example we are going to use Unity to download and to test our program
set(UNITY_GIT_URL "https://github.com/ThrowTheSwitch/Unity.git")

# Enable debug
set(FETCHCONTENT_QUIET OFF)

FetchContent_Declare(
    Unity
    GIT_REPOSITORY ${UNITY_GIT_URL}
    SOURCE_DIR ${CMAKE_CURRENT_SOURCE_DIR}/external/Unity
    UPDATE_COMMAND "" # Disable the update command
)

FetchContent_MakeAvailable(Unity)

# Add the 'src' directory, where the source files are located
file(GLOB_RECURSE SOURCES "src/*.c")

# Create the executable 
add_executable(${PROJECT_NAME} ${SOURCES})

# Compile the libraries
add_subdirectory(lib/libdyn)
add_subdirectory(lib/libsta)

# Include headers in lib
target_include_directories(${PROJECT_NAME} PUBLIC lib/libdyn/include)
target_include_directories(${PROJECT_NAME} PUBLIC lib/libsta/include)

# Link the libraries
target_link_libraries(${PROJECT_NAME} StatiCLib DynamiCLib)

# Add subdirectory of test
if(RUN_TEST EQUAL 1 OR RUN_COVERAGE EQUAL 1)
    add_subdirectory(tests)
endif()
