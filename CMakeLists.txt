cmake_minimum_required(VERSION 3.0 )
project(bonefish)

option(shared "build bonefish as a shared library" OFF)
option(stdlib "When building with clang, you can choose libc++ (default) or libstdc++" OFF)

set(BOOST_COMPONENTS program_options system thread)

add_library( bonefish_common INTERFACE )

set(CMAKE_MODULE_PATH
    ${CMAKE_MODULE_PATH}
    "${CMAKE_CURRENT_SOURCE_DIR}/cmake/Modules/")

if(shared)
    set(Boost_USE_STATIC_LIBS OFF)
    set(RT_USE_STATIC_LIBS OFF)
else()
    set(Boost_USE_STATIC_LIBS ON)
    set(RT_USE_STATIC_LIBS ON)
endif()

if(WIN32)
    add_definitions(-D_WIN32_WINNT=0x0500)
    add_definitions(-DWIN32_LEAN_AND_MEAN)
endif()

find_package(Boost REQUIRED COMPONENTS ${BOOST_COMPONENTS})
find_package(RT)
find_package(Threads)

include_directories(
    ${CMAKE_CURRENT_SOURCE_DIR}/src
    ${CMAKE_CURRENT_SOURCE_DIR}/third-party/cppcodec
    ${CMAKE_CURRENT_SOURCE_DIR}/third-party/json-msgpack/include
    ${CMAKE_CURRENT_SOURCE_DIR}/third-party/msgpack-c/include
    ${CMAKE_CURRENT_SOURCE_DIR}/third-party/rapidjson/include
    ${CMAKE_CURRENT_SOURCE_DIR}/third-party/websocketpp
    ${Boost_INCLUDE_DIRS}
)

link_directories(${CMAKE_INSTALL_PREFIX}/lib)

add_subdirectory(examples)
add_subdirectory(src)
add_subdirectory(daemon)
add_subdirectory(test)
