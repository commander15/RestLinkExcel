qt_add_library(
    RestLinkExcelLib STATIC
    worker.h worker.cpp
    documentmanager.h documentmanager.cpp
    document.h document.cpp
)

target_include_directories(
    RestLinkExcelLib
    PUBLIC
        $<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}>
)

target_link_libraries(
    RestLinkExcelLib
    PUBLIC
        RestLink::RestLink
        QXlsx
)
