# Reference: http://qt-project.org/doc/qt-5.0/qtdoc/cmake-manual.html

cmake_minimum_required(VERSION 2.8.12)
cmake_policy(SET CMP0020 NEW)

set(PROJECT "example")

project(${PROJECT})

set(CMAKE_MODULE_PATH ${CMAKE_CURRENT_SOURCE_DIR}/cmake)
set(CMAKE_INCLUDE_CURRENT_DIR ON)
set(CMAKE_AUTOMOC ON)

find_package(Qt5Widgets REQUIRED)

set(SOURCE_FILES
    src/main.cc
    src/mainwindow.h
    src/mainwindow.cc
)

if (WIN32)
    set(SOURCE_FILES ${SOURCE_FILES} win/example.rc)
endif ()

set(UI_FILES
    ui/mainwindow.ui
)
source_group("UI Files" FILES ${UI_FILES})

qt5_wrap_ui(UI_HEADERS ${UI_FILES})
source_group("Generated UI Headers" FILES ${UI_HEADERS})

qt5_add_resources(RESOURCE_FILES example.qrc)

if (APPLE)
    set(SOURCE_FILES ${SOURCE_FILES} AppIcon.icns)
    set_source_files_properties(AppIcon.icns PROPERTIES MACOSX_PACKAGE_LOCATION Resources)
endif ()

add_executable(${PROJECT} MACOSX_BUNDLE
    ${SOURCE_FILES}
    ${UI_HEADERS}
    ${UI_FILES}
    ${RESOURCE_FILES}
)

set(MACOSX_BUNDLE_BUNDLE_NAME "Example")
set(MACOSX_BUNDLE_BUNDLE_VERSION "1.0")
set(MACOSX_BUNDLE_LONG_VERSION_STRING "1.0")
set(MACOSX_BUNDLE_SHORT_VERSION_STRING "1.0")
set(MACOSX_BUNDLE_COPYRIGHT "Copyright (c) 2014 Vincent Lee. All rights reserved.")
set(MACOSX_BUNDLE_GUI_IDENTIFIER "com.example.Example")
set(MACOSX_BUNDLE_ICON_FILE AppIcon.icns)

qt5_use_modules(${PROJECT} Widgets)

if (MSVC)
    set_target_properties(${PROJECT} PROPERTIES
        WIN32_EXECUTABLE YES
        LINK_FLAGS "/ENTRY:mainCRTStartup"
    )
endif ()
