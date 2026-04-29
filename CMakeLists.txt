cmake_minimum_required(VERSION 3.30)

project(RestLinkExcel
    VERSION 1.0.0
)

option(RESTLINK_EXCEL_BUILD_TESTS "Build plugin tests" OFF)

# Qt

set(QT_MODULES Core)
if (PROJECT_IS_TOP_LEVEL)
    set(CMAKE_AUTOMOC ON)
    set(CMAKE_AUTORCC ON)

    list(APPEND QT_MODULES Network)
endif()

if (RESTLINK_EXCEL_BUILD_TESTS)
    list(APPEND QT_MODULES Test)
endif()

find_package(Qt6 REQUIRED COMPONENTS ${QT_MODULES})

# RestLink

set(RESTLINK_REQUIRED_VERSION 2.3.1)

if (PROJECT_IS_TOP_LEVEL)
    option(RESTLINK_BUILD_CLI     OFF)
    option(RESTLINK_BUILD_QML     OFF)
    option(RESTLINK_SUPPORT_EXCEL OFF)

    find_package(RestLink ${RESTLINK_REQUIRED_VERSION} REQUIRED)
elseif (TARGET RestLink)
    if (${RestLink_VERSION} VERSION_GREATER_EQUAL ${RESTLINK_REQUIRED_VERSION})
        if (NOT TARGET RestLink::RestLink)
            add_library(RestLink::RestLink ALIAS RestLink)
        endif()
    else()
        message(FATAL_ERROR "RestLink Excel requires at least RestLink ${RESTLINK_REQUIRED_VERSION}")
    endif()
else()
    message(FATAL_ERROR "RestLink Excel: can't find RestLink core library")
endif()

# QXlsx

include(FetchContent)

FetchContent_Declare(
    QXlsx
    GIT_REPOSITORY "https://github.com/QtExcel/QXlsx.git"
    GIT_TAG        "v1.5.0"
    SOURCE_SUBDIR QXlsx
)

FetchContent_MakeAvailable(QXlsx)
if (TARGET QXlsx AND NOT TARGET QXlsx::QXlsx)
    add_library(QXlsx::QXlsx ALIAS QXlsx)
endif()

add_subdirectory(src)
add_subdirectory(plugin)

if (RESTLINK_EXCEL_BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif()
