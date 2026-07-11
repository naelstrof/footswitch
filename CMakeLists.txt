cmake_minimum_required(VERSION 3.13)
project(footswitch C)

set(CMAKE_RUNTIME_OUTPUT_DIRECTORY build)
set(SRCDIR src)

find_package(PkgConfig REQUIRED)
pkg_check_modules(HIDAPI REQUIRED IMPORTED_TARGET hidapi-libusb)

foreach(exe IN ITEMS footswitch scythe scythe2 footswitch1p)
  add_executable(${exe}
    ${SRCDIR}/common.c
    ${SRCDIR}/debug.c
    ${SRCDIR}/${exe}.c
  )

  target_include_directories(${exe} PRIVATE include)
  target_link_libraries(${exe} PRIVATE PkgConfig::HIDAPI)

  install(TARGETS ${exe}
    RUNTIME DESTINATION bin
  )
endforeach()
