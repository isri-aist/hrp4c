cmake_minimum_required(VERSION 2.4)

execute_process(
  COMMAND pkg-config --variable=prefix openhrp3.1
  OUTPUT_VARIABLE OPENHRP_DIR
  RESULT_VARIABLE RESULT
  OUTPUT_STRIP_TRAILING_WHITESPACE)

set(CMAKE_INSTALL_PREFIX ${OPENHRP_DIR})

set(CPACK_PACKAGE_NAME hrp4c)
set(CPACK_PACKAGE_VENDOR "AIST")
set(CPACK_PACKAGE_CONTACT "Fumio Kanehiro <f-kanehiro@aist.go.jp>")
set(CPACK_PACKAGE_VERSION_MAJOR 1)
set(CPACK_PACKAGE_VERSION_MINOR 0)
set(CPACK_PACKAGE_VERSION_PATCH 0)

execute_process(
  COMMAND date +%Y%m%d%H%M
  OUTPUT_VARIABLE CPACK_PACKAGE_DATE
  RESULT_VARIABLE RESULT
  OUTPUT_STRIP_TRAILING_WHITESPACE)

set(CPACK_PACKAGE_VERSION ${CPACK_PACKAGE_VERSION_MAJOR}.${CPACK_PACKAGE_VERSION_MINOR}.${CPACK_PACKAGE_VERSION_PATCH}-${CPACK_PACKAGE_DATE})
set(CPACK_DEBIAN_PACKAGE_ARCHITECTURE all)
set(CPACK_PACKAGE_FILE_NAME "${CPACK_PACKAGE_NAME}-${CPACK_PACKAGE_VERSION}${OPENHRP_VERSION_ADD_TAIL}_${CPACK_DEBIAN_PACKAGE_ARCHITECTURE}")
set(CPACK_PACKAGING_INSTALL_PREFIX "/usr")

include(CPack)

#add_subdirectory(customizer)
add_subdirectory(model)

configure_file(hrp4c.pc.in ${CMAKE_CURRENT_BINARY_DIR}/hrp4c.pc @ONLY)
install(FILES
  ${CMAKE_CURRENT_BINARY_DIR}/hrp4c.pc
  DESTINATION lib/pkgconfig)
