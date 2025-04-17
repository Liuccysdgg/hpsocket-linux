cmake_minimum_required(VERSION 3.10)
project(hpsocket)

set(CMAKE_CXX_STANDARD 17)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

file(GLOB_RECURSE ALL_SRC_FILES CONFIGURE_DEPENDS
    "${CMAKE_CURRENT_SOURCE_DIR}/*.cpp"
    "${CMAKE_CURRENT_SOURCE_DIR}/*.c"
)

add_library(hpsocket STATIC ${ALL_SRC_FILES})
target_include_directories(hpsocket PUBLIC ${CMAKE_CURRENT_SOURCE_DIR})

# 解包 brotli 的 .a 为 .o 文件
file(MAKE_DIRECTORY ${CMAKE_BINARY_DIR}/brotli_objects)
execute_process(
    COMMAND ${CMAKE_AR} -x ${CMAKE_CURRENT_SOURCE_DIR}/libbrotli.a
    WORKING_DIRECTORY ${CMAKE_BINARY_DIR}/brotli_objects
)

file(GLOB BROTLI_OBJECTS "${CMAKE_BINARY_DIR}/brotli_objects/*.o")
target_sources(hpsocket PRIVATE ${BROTLI_OBJECTS})
