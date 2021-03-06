project(UnscentedKF)

cmake_minimum_required (VERSION 3.5)

add_definitions(-std=c++11)

set(CXX_FLAGS "-Wall")
set(CMAKE_CXX_FLAGS, "${CXX_FLAGS}")

set(sources src/ukf.cpp src/tools.cpp)
set(test_sources test/setup.cpp test/test_tools.cpp test/test_ukf.cpp)

if(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")

include_directories(/usr/local/include)
include_directories(/usr/local/opt/openssl/include)
link_directories(/usr/local/lib)
link_directories(/usr/local/opt/openssl/lib)
link_directories(/usr/local/Cellar/libuv/1*/lib)

endif(${CMAKE_SYSTEM_NAME} MATCHES "Darwin")

# Add source files to a library to avoid additional compile for unit tests.
add_library (UnscentedKF_lib ${sources})

# Target app
add_executable(UnscentedKF src/main.cpp)
target_link_libraries(UnscentedKF UnscentedKF_lib z ssl uv uWS)

# Unit tests
add_executable(UnscentedKF_unit_tests ${test_sources})
target_link_libraries(UnscentedKF_unit_tests UnscentedKF_lib)
enable_testing()
add_test(NAME ExtendedKF_Test_Suite COMMAND UnscentedKF_unit_tests)

# Tuning app
add_executable(UnscentedKF_tuning src/main_tuning.cpp)
target_link_libraries(UnscentedKF_tuning UnscentedKF_lib)

# Catch the run away car
add_executable(CatchTheRunAwayCar src/main_catch_the_run_away_car.cpp)
target_link_libraries(CatchTheRunAwayCar UnscentedKF_lib z ssl uv uWS)
