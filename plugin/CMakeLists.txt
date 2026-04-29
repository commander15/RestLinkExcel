restlink_add_plugin(
    RestLinkExcel
    CLASS_NAME RestLink::Excel::Plugin
    SOURCES plugin.cpp
    LINK_LIBRARIES RestLinkExcelLib
)

restlink_install_plugin(RestLinkExcelLib)
