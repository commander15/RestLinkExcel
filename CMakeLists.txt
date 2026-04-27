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

if (PROJECT_IS_TOP_LEVEL)
    option(RESTLINK_BUILD_CLI     OFF)
    option(RESTLINK_BUILD_QML     OFF)
    option(RESTLINK_SUPPORT_EXCEL OFF)

    find_package(RestLink 2.3.0 REQUIRED)
elseif (TARGET RestLink)
    add_library(RestLink::RestLink ALIAS RestLink)
else()
    message(FATAL_ERROR "Can't find RestLink core library")
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
