cmake_minimum_required(VERSION 3.24)
project(mediasoup LANGUAGES C CXX VERSION 3.12.11) #todo from git

option(MEDIASOUP_BUILD_TESTS "Build mediasoup tests" OFF)

if (MEDIASOUP_BUILD_TESTS)
    include(CTest)
endif ()

add_subdirectory(worker)