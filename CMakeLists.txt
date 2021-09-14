cmake_minimum_required(VERSION 3.1)

project (saned-epkowa)

file(GLOB BackendSources src/*.c)
file(GLOB SaneiSources sanei/src/*.c)
file(GLOB UtilsSources utils/*.c)

add_library(sane-epkowa ${BackendSources} ${SaneiSources} ${UtilsSources})

target_include_directories(sane-epkowa PRIVATE sanei/include)
target_include_directories(sane-epkowa PRIVATE utils)

target_link_libraries(sane-epkowa ltdl usb sane)

target_compile_definitions(sane-epkowa PUBLIC HAVE_LIBUSB_1_0)