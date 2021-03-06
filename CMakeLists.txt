cmake_minimum_required(VERSION 3.1)

project(sane-epkowa)

set(PKG_NAME "epkowa" CACHE STRING "")
set(MODEL_DATA_DIR_NAME "device" CACHE STRING "")
set(LIB_DIR "/usr/lib/sane" CACHE STRING "")
set(STATE_DIR "/usr/lib/sane" CACHE STRING "")
set(DATA_DIR "/usr/share/sane" CACHE STRING "")
set(PKG_DATA_DIR "${DATA_DIR}/${PKG_NAME}" CACHE STRING "")
set(PKG_LIB_DIR "${LIB_DIR}/${PKG_NAME}" CACHE STRING "")
set(PKG_STATE_DIR "${STATE_DIR}/${PKG_NAME}" CACHE STRING "")
set(MODEL_DATA_DIR "${PKG_DATA_DIR}/${MODEL_DATA_DIR_NAME}" CACHE STRING "")

find_package(LibXml2 REQUIRED)

file(GLOB BackendSources backend/*.c)
file(GLOB SaneiSources sanei/src/*.c)

add_library(sane-epkowa MODULE ${BackendSources} ${SaneiSources})

target_include_directories(sane-epkowa PUBLIC ${LIBXML2_INCLUDE_DIRS})
target_include_directories(sane-epkowa PRIVATE sanei)
target_include_directories(sane-epkowa PRIVATE sanei/include)

target_link_libraries(sane-epkowa ltdl usb-1.0 sane)
target_link_libraries(sane-epkowa ${LIBXML2_LIBRARIES})

target_compile_definitions(sane-epkowa PUBLIC BACKEND_NAME=epkowa)
target_compile_definitions(sane-epkowa PUBLIC SANE_MAJOR=1)
target_compile_definitions(sane-epkowa PUBLIC SANE_MINOR=0)
target_compile_definitions(sane-epkowa PUBLIC SANE_REVISION=32)
target_compile_definitions(sane-epkowa PUBLIC PKGDATADIR="${PKG_DATA_DIR}")
target_compile_definitions(sane-epkowa PUBLIC MODELDATADIR="${MODEL_DATA_DIR}")
target_compile_definitions(sane-epkowa PUBLIC PKGLIBDIR="${PKG_LIB_DIR}")
target_compile_definitions(sane-epkowa PUBLIC PKGLOCALSTATEDIR="${PKG_STATE_DIR}")
target_compile_definitions(sane-epkowa PUBLIC HAVE_LIBUSB_1_0)

install(TARGETS sane-epkowa LIBRARY DESTINATION lib/sane)
install(FILES data/04b8-epkowa.usbid DESTINATION share/sane)
install(DIRECTORY data/device/ DESTINATION share/sane/${PKG_NAME}/${MODEL_DATA_DIR_NAME})
install(FILES data/epkowa.conf DESTINATION etc/sane.d)
Install(FILES data/dll.conf DESTINATION etc/sane.d/dll.d RENAME epkowa)