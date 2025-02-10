cmake_minimum_required(VERSION 3.5)

set(PROJECT_NAME YOLOv8CPP)
project(${PROJECT_NAME} VERSION 0.0.1 LANGUAGES CXX)

# set(CMAKE_CXX_STANDARD 17)
# set(CMAKE_INCLUDE_CURRENT_DIR ON)

set(CMAKE_CXX_STANDARD_REQUIRED ON)
set(CMAKE_CXX_EXTENSIONS ON)
set(ONNXRUNTIME_VERSION 1.20.1)

find_package(OpenCV REQUIRED)
if(NOT OpenCV_FOUND)
    message(FATAL_ERROR "[${PROJECT_NAME}] OpenCV found failed")
endif()

file(GLOB_RECURSE PROJECT_SOURCES src/*.cpp)

if (LINUX)
    set(CMAKE_OSX_ARCHITECTURES "x86_64")
elseif (APPLE)
    set(CMAKE_OSX_ARCHITECTURES "arm64")
    set(ONNXRUNTIME_DIR "/Users/miguel/idwall/iq-lib/onnx-${ONNXRUNTIME_VERSION}")  #
    set(ONNXRUNTIME_LIB ${ONNXRUNTIME_DIR}/lib/libonnxruntime.dylib)                #
endif ()

message(STATUS
    "[${PROJECT_NAME}]
    OpenCV_VERSION: ${OpenCV_VERSION},
    OpenCV_DIR: ${OpenCV_DIR}"
)

include_directories(
    ${PROJECT_NAME} PRIVATE
    include
    ${OpenCV_INCLUDE_DIRS}
    ${ONNXRUNTIME_DIR}/include
)

add_executable(
    ${PROJECT_NAME}
    ${PROJECT_SOURCES}
)

target_link_libraries(
    ${PROJECT_NAME} PRIVATE
    ${OpenCV_LIBS}
    ${ONNXRUNTIME_LIB}
)

target_compile_features(${PROJECT_NAME} PRIVATE cxx_std_17)
