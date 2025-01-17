cmake_minimum_required(VERSION 3.10)
project(bank)
set (CMAKE_CXX_STANDARD 17)

if(NOT CMAKE_BUILD_TYPE)
set(CMAKE_BUILD_TYPE Release)
endif()

set(CMAKE_CXX_FLAGS "-Wall -Wextra")
set(CMAKE_CXX_FLAGS_DEBUG "-g")
set(CMAKE_CXX_FLAGS_RELEASE "-O3")

find_package(Threads REQUIRED)
add_executable(${PROJECT_NAME} main.cpp )
target_sources(${PROJECT_NAME} PRIVATE
    src/xxhash.c
    src/bank_f.cpp
    src/bank.cpp
    src/log.cpp
    src/transaction.cpp
    src/user.cpp
)

target_include_directories(${PROJECT_NAME} PUBLIC include)
target_include_directories(${PROJECT_NAME} PUBLIC third_party)
target_include_directories(${PROJECT_NAME} PUBLIC third_party/drogon/lib/inc)

add_subdirectory(third_party/drogon)
target_link_libraries(${PROJECT_NAME} PRIVATE drogon)
target_link_libraries(${PROJECT_NAME} PRIVATE ${CMAKE_THREAD_LIBS_INIT} )
