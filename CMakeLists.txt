cmake_minimum_required(VERSION 3.0)
project(fisheye_stereo)

#set(CMAKE_CXX_STANDARD 23)
set(CMAKE_CXX_STANDARD 20)
# set(CMAKE_BUILD_TYPE Debug)
set(CMAKE_BUILD_TYPE Release)
set(CMAKE_CXX_FLAGS_RELEASE "${CMAKE_CXX_FLAGS_RELEASE} -O3")
set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -march=native -pthread")

find_package(OpenCV REQUIRED)
find_package(PCL REQUIRED)

include_directories(
    include
    ${OpenCV_INCLUDE_DIRS}
    ${PCL_INCLUDE_DIRS}
)

add_library(stereo_matcher
    src/engines/stereo_matcher.cpp
    src/configs/stereo_matcher_params.cpp
    src/utils/config_utils.cpp
)

add_library(pcd_generator
    src/engines/pcd_generator.cpp
    src/configs/pcd_generator_params.cpp
    src/utils/config_utils.cpp
)

target_link_libraries(stereo_matcher ${OpenCV_LIBS} stdc++fs)
target_link_libraries(pcd_generator ${OpenCV_LIBS} ${PCL_LIBRARIES} stdc++fs)

add_executable(calibration
    app/calibration.cpp
    src/engines/calibration_model.cpp
    src/configs/calibration_model_params.cpp
    src/utils/config_utils.cpp
    src/utils/undistort_utils.cpp
)
target_link_libraries(calibration ${OpenCV_LIBS})

add_executable(realtime_stereo
    app/realtime_stereo.cpp
    src/stereo_vision.cpp
    src/configs/stereo_vision_params.cpp
    src/utils/undistort_utils.cpp
)
target_link_libraries(realtime_stereo stereo_matcher pcd_generator ${OpenCV_LIBS} ${PCL_LIBRARIES} stdc++fs)

add_executable(test
    app/test.cpp
    src/utils/config_utils.cpp
    src/utils/undistort_utils.cpp
)
target_link_libraries(test stereo_matcher pcd_generator ${OpenCV_LIBS} ${PCL_LIBRARIES})

add_executable(test_pair
    app/test_pair.cpp
    src/utils/config_utils.cpp
    src/utils/undistort_utils.cpp
)
target_link_libraries(test_pair stereo_matcher pcd_generator ${OpenCV_LIBS} ${PCL_LIBRARIES})

add_executable(compare_fisheye
    app/compare_fisheye.cpp
    src/utils/config_utils.cpp
    src/utils/undistort_utils.cpp
)
target_link_libraries(compare_fisheye stereo_matcher pcd_generator ${OpenCV_LIBS} ${PCL_LIBRARIES})
