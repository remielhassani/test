cmake_minimum_required(VERSION 3.17)

project(
  cyclicapp
  VERSION 0.1.0
  LANGUAGES CXX)

if(NOT CMAKE_BUILD_TYPE AND NOT CMAKE_CONFIGURATION_TYPES)
  message(STATUS "Setting build type to 'Release' as none was specified.")
  set(CMAKE_BUILD_TYPE
      Release
      CACHE STRING "Choose the type of build." FORCE)
  set_property(CACHE CMAKE_BUILD_TYPE PROPERTY STRINGS "Debug" "Release")
endif()

# if the app shall require a dependency like mosquitto the following two lines can be uncommented
# set(CONAN_REQUIRES mosquitto/2.0.10)
# set(CONAN_OPTIONS mosquitto:shared=True)

if(NOT EXISTS "${CMAKE_BINARY_DIR}/conan.cmake")
  message(STATUS "Downloading conan.cmake from https://github.com/conan-io/cmake-conan")
  file(DOWNLOAD "https://github.com/conan-io/cmake-conan/raw/v0.15/conan.cmake"
       "${CMAKE_BINARY_DIR}/conan.cmake")
endif()

include(${CMAKE_BINARY_DIR}/conan.cmake)

conan_cmake_run(
  REQUIRES
  ${CONAN_REQUIRES}
  OPTIONS
  ${CONAN_OPTIONS}
  BASIC_SETUP
  CMAKE_TARGETS
  BUILD
  missing)

include(${CMAKE_BINARY_DIR}/conanbuildinfo.cmake)
conan_basic_setup()

add_library(${PROJECT_NAME} SHARED src/app.cpp)

# the following line links dependencies like mosquitto to your target, uncomment if needed
# target_link_libraries(${PROJECT_NAME} PRIVATE CONAN_LIB::mosquitto_mosquitto)

target_include_directories(${PROJECT_NAME} PRIVATE /home/vscode/mosaiq/sdk/include)
