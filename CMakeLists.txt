cmake_minimum_required(VERSION 3.1)

option(serial_BUILD_EXAMPLE "build example" OFF)

if (CMAKE_CURRENT_SOURCE_DIR STREQUAL CMAKE_SOURCE_DIR)
    set(serial_BUILD_EXAMPLE ON)
endif ()

project(serial)

if (APPLE)
    find_library(IOKIT_LIBRARY IOKit)
    find_library(FOUNDATION_LIBRARY Foundation)
endif ()

## Include headers
include_directories(include)

## Sources
set(serial_SRCS
        src/serial.cc
        include/serial/serial.h
        include/serial/v8stdint.h
        )
if (APPLE)
    # If OSX
    list(APPEND serial_SRCS src/impl/unix.cc)
    list(APPEND serial_SRCS src/impl/list_ports/list_ports_osx.cc)
elseif (UNIX)
    # If unix
    list(APPEND serial_SRCS src/impl/unix.cc)
    list(APPEND serial_SRCS src/impl/list_ports/list_ports_linux.cc)
else ()
    # If windows
    list(APPEND serial_SRCS src/impl/win.cc)
    list(APPEND serial_SRCS src/impl/list_ports/list_ports_win.cc)
endif ()

## Add serial library
add_library(${PROJECT_NAME} ${serial_SRCS})
if (APPLE)
    target_link_libraries(${PROJECT_NAME} ${FOUNDATION_LIBRARY} ${IOKIT_LIBRARY})
elseif (UNIX)
    target_link_libraries(${PROJECT_NAME} rt pthread)
else ()
    target_link_libraries(${PROJECT_NAME} setupapi)
endif ()

if (serial_BUILD_EXAMPLE)
    add_executable(serial_example examples/serial_example.cc)
    add_dependencies(serial_example ${PROJECT_NAME})
    target_link_libraries(serial_example ${PROJECT_NAME})
endif ()
