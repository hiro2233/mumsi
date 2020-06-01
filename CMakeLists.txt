cmake_minimum_required(VERSION 2.8.1)
project(mumsi)

set(CMAKE_CXX_FLAGS "${CMAKE_CXX_FLAGS} -std=c++11 -pthread")

INCLUDE(FindPkgConfig)
find_package(Boost COMPONENTS system regex REQUIRED)
find_package(PkgConfig REQUIRED)

#pkg_check_modules(PJSIP "libpjproject>=2.6")
pkg_check_modules(LOG4CPP "log4cpp")

add_definitions(${PJSIP_CFLAGS})
add_definitions(${LOG4CPP_CFLAGS})

include_directories(${PJSIP_INCLUDE_DIRS})
include_directories(${LOG4CPP_INCLUDE_DIRS})
include_directories(../mumpi/deps/mumlib/include)
include_directories(../pjproject/install/include)

link_directories(../mumpi/build/deps/mumlib)
link_directories(../pjproject/install/lib)

set(SOURCE_FILES
        PjsuaCommunicator.cpp
        PjsuaCommunicator.hpp
        MumbleCommunicator.cpp
        MumbleCommunicator.hpp
        MumbleChannelJoiner.cpp
        MumbleChannelJoiner.hpp
        Configuration.cpp
        Configuration.hpp
        IncomingConnectionValidator.cpp
        IncomingConnectionValidator.hpp
        AudioFramesMixer.hpp
        AudioFramesMixer.cpp)

add_executable(mumsi ${SOURCE_FILES} main.cpp)
target_link_libraries(mumsi pjsua2 pjsua pjmedia pj)
target_link_libraries(mumsi ${LOG4CPP_LIBRARIES})
target_link_libraries(mumsi ${Boost_LIBRARIES})
target_link_libraries(mumsi mumlib)

