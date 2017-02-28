cmake_minimum_required(VERSION 3.0)

project(PulseCounter CXX)

find_path(LIBUSB_INCLUDE_DIR usb.h)
find_library(LIBUSB_LIBRARY usb)
set(LIBUSB_FOUND LIBUSB_INCLUDE_DIR AND LIBUSB_LIBRARY)
if (LIBUSB_FOUND)
  message(STATUS "+ libusb")
  message(STATUS "  include: ${LIBUSB_INCLUDE_DIR}")
  message(STATUS "      lib: ${LIBUSB_LIBRARY}")
else()
  message(FATAL_ERROR "Couldn't find libusb-0.1")
endif ()

include_directories(${LIBUSB_INCLUDE_DIR})

add_executable(cycfx2prog
  cycfx2/cycfx2prog.cc
  cycfx2/cycfx2dev.cc
)

target_compile_definitions(cycfx2prog PRIVATE -DCYCFX2PROG_VERSION=\"0.47\")
target_link_libraries(cycfx2prog ${LIBUSB_LIBRARY})