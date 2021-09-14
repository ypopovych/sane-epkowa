cmake_minimum_required(VERSION 3.1)

project(sane-epkowa)

set(PKG_NAME "epkowa" CACHE STRING "")
set(MODEL_DATA_DIR_NAME "device" CACHE STRING "")
set(LIB_DIR "/usr/lib/sane" CACHE STRING "")
set(STATE_DIR "/var/lib/sane" CACHE STRING "")
set(DATA_DIR "/usr/share/sane" CACHE STRING "")
set(PKG_DATA_DIR "${DATA_DIR}/${PKG_NAME}" CACHE STRING "")
set(PKG_LIB_DIR "${LIB_DIR}/${PKG_NAME}" CACHE STRING "")
set(PKG_STATE_DIR "${STATE_DIR}/${PKG_NAME}" CACHE STRING "")
set(MODEL_DATA_DIR "${PKG_DATA_DIR}/${MODEL_DATA_DIR_NAME}" CACHE STRING "")


file(GLOB BackendSources backend/*.c)
file(GLOB SaneiSources sanei/src/*.c)
file(GLOB UtilsSources utils/*.c)

add_library(sane-epkowa ${BackendSources} ${SaneiSources} ${UtilsSources})

target_include_directories(sane-epkowa PRIVATE sanei)
target_include_directories(sane-epkowa PRIVATE sanei/include)
target_include_directories(sane-epkowa PRIVATE utils)

target_link_libraries(sane-epkowa ltdl usb-1.0 sane)

target_compile_definitions(sane-epkowa PUBLIC BACKEND_NAME=epkowa)
target_compile_definitions(sane-epkowa PUBLIC SANE_MAJOR=1)
target_compile_definitions(sane-epkowa PUBLIC SANE_MINOR=0)
target_compile_definitions(sane-epkowa PUBLIC SANE_REVISION=32)
target_compile_definitions(sane-epkowa PUBLIC PKGDATADIR="${PKG_DATA_DIR}")
target_compile_definitions(sane-epkowa PUBLIC MODELDATADIR="${MODEL_DATA_DIR}")
target_compile_definitions(sane-epkowa PUBLIC PKGLIBDIR="${PKG_LIB_DIR}")
target_compile_definitions(sane-epkowa PUBLIC PKGLOCALSTATEDIR="${PKG_STATE_DIR}")
target_compile_definitions(sane-epkowa PUBLIC HAVE_LIBUSB_1_0)