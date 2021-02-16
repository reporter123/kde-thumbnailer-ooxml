project(kde-thumbnailer-ooxml)
cmake_minimum_required(VERSION 3.6 FATAL_ERROR)
set(QT_MIN_VERSION "5.3.0")

find_package(ECM 1.0.0 REQUIRED NO_MODULE)
set(CMAKE_MODULE_PATH ${ECM_MODULE_PATH} ${ECM_KDE_MODULE_DIR})

include(KDEInstallDirs)
include(KDECMakeSettings)
include(KDECompilerSettings)

find_package(Qt5 ${QT_MIN_VERSION} CONFIG REQUIRED COMPONENTS Gui Xml)
find_package(KF5 REQUIRED COMPONENTS KIO Archive)

add_definitions(${QT_DEFINITIONS} ${KF5_DEFINITIONS} -DQT_USE_FAST_CONCATENATION -DQT_USE_FAST_OPERATOR_PLUS)

include_directories(${QT_INCLUDES} ${KF5_INCLUDES} ${Qt5Xml_INCLUDE_DIRS})

set(libkowmf_SRCS
    libkowmf/WmfAbstractBackend.cpp
    libkowmf/WmfDeviceContext.cpp
    libkowmf/WmfPainterBackend.cpp
    libkowmf/WmfParser.cpp
    libkowmf/WmfStack.cpp
    libkowmf/libkowmflogging.cpp
)

set(ooxmlthumbnail_SRCS ooxmlcreator.cpp ${libkowmf_SRCS})

add_library(ooxmlthumbnail ${ooxmlthumbnail_SRCS})
target_link_libraries(ooxmlthumbnail
    Qt5::Gui Qt5::Xml KF5::KIOWidgets KF5::Archive
)

install(TARGETS ooxmlthumbnail DESTINATION ${PLUGIN_INSTALL_DIR})
install(FILES ooxmlthumbnail.desktop DESTINATION ${SERVICES_INSTALL_DIR})
