cmake_minimum_required(VERSION 3.0 FATAL_ERROR)
project(test02)

set(CMAKE_PREFIX_PATH C:\\libtorch\\share\\cmake\\Torch)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} ${TORCH_CXX_FLAGS}")
set(OpenCV_DIR C:\\opencv\\build\\x64\\vc15\\lib)


find_package(Torch REQUIRED)
find_package(OpenCV REQUIRED)
#include_directories(${OpenCV_INCLUDE_DIRS})

add_executable(test02 main.cpp utils.hpp detector.hpp service.hpp m.h)
target_link_libraries(test02  ${OpenCV_LIBS} ${TORCH_LIBRARIES} ws2_32)
set_property(TARGET test02 PROPERTY CXX_STANDARD 11)

if (MSVC)
    file(GLOB TORCH_DLLS "${TORCH_INSTALL_PREFIX}/lib/*.dll")
    add_custom_command(TARGET test02
            POST_BUILD
            COMMAND ${CMAKE_COMMAND} -E copy_if_different
            ${TORCH_DLLS}
            $<TARGET_FILE_DIR:test02>)

endif (MSVC)
