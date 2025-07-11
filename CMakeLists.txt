
# 指定cmake最小版本
cmake_minimum_required(VERSION 3.20)

# 指定编译的C++标准版本
#set(CMAKE_CXX_STANDARD 11)
# 设置是否强制要求使用指定的版本， OFF时可能会自动将低版本
#set(CMAKE_CXX_STANDARD_REQUIRED ON)

# 指定工程名
project(cmake_sample VERSION 0.01)

# 替换变量复制文件
configure_file(config.h.in ${CMAKE_SOURCE_DIR}/config.h)
message(NOTICE "${PROJECT_SOURCE_DIR}")

# 用指定的源文件创建一个可执行文件
add_executable(cmake_sample main.cpp)

# 头文件包含
target_include_directories(cmake_sample PUBLIC ${PROJECT_SOURCE_DIR} ${PROJECT_BINARY_DIR})

add_subdirectory(${PROJECT_SOURCE_DIR}/mathematic)

# 链接库
target_link_libraries(cmake_sample PRIVATE mathematic)

add_library(cmake_sample_flags INTERFACE)
target_compile_features(cmake_sample_flags INTERFACE cxx_std_11)
target_link_libraries(cmake_sample PUBLIC cmake_sample_flags)


set(set_test "set_test" CACHE STRING "this is a test" FORCE)


# enable cmake_file_api
set(CMAKE_EXPORT_COMPILE_COMMANDS ON)
set(CMAKE_FILE_API_VERSION 1)


#[[
comment
]]

# This is a line comment.
message("First Argument\n" # This is a line comment :)
        "Second Argument") # This is a line comment.






