set(INSTALL_DIR plugins/restlink)
set(OUTPUT_DIR ${CMAKE_BINARY_DIR}/${INSTALL_DIR})

qt_add_plugin(RestLinkExcel
    CLASS_NAME RestLink::Excel::Plugin
    plugin.cpp
)

target_sources(RestLinkExcel
  PRIVATE
    worker.h worker.cpp
    documentmanager.h documentmanager.cpp
    document.h document.cpp
)

set_target_properties(RestLinkExcel PROPERTIES
    OUTPUT_NAME restlinkexcel
    PREFIX      ""
    ARCHIVE_OUTPUT_DIRECTORY ${OUTPUT_DIR}
    LIBRARY_OUTPUT_DIRECTORY ${OUTPUT_DIR}
    RUNTIME_OUTPUT_DIRECTORY ${OUTPUT_DIR}
)

target_link_libraries(
    RestLinkExcel
    PUBLIC
        RestLink::RestLink
        QXlsx::QXlsx
)

install(
    TARGETS RestLinkExcel
    ARCHIVE DESTINATION ${INSTALL_DIR}
    LIBRARY DESTINATION ${INSTALL_DIR}
    RUNTIME DESTINATION ${INSTALL_DIR}
)
