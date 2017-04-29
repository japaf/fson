set (CMAKE_FORTRAN_FLAGS "-O3")

file (GLOB SRCFiles *.f*)

set (CMAKE_Fortran_MODULE_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/mod)

add_library(fson SHARED ${SRCFiles})

# --------------------------------------------------------------------------- #
# Generate Configuration Files
# --------------------------------------------------------------------------- #

configure_file("${CMAKE_CURRENT_SOURCE_DIR}/libfson.pc.in" "${CMAKE_CURRENT_BINARY_DIR}/libfson.pc" @ONLY)


install(
	TARGETS fson
	RUNTIME DESTINATION "${INSTALL_BIN_DIR}"
	LIBRARY DESTINATION "${INSTALL_LIB_DIR}/${CMAKE_PROJECT_NAME}"
	ARCHIVE DESTINATION "${INSTALL_LIB_DIR}/static"
	PUBLIC_HEADER DESTINATION "${INSTALL_INCLUDE_DIR}/${CMAKE_PROJECT_NAME}"
)


install(FILES "${CMAKE_CURRENT_BINARY_DIR}/libfson.pc" DESTINATION "${INSTALL_LIB_DIR}/pkgconfig")

install(CODE "EXECUTE_PROCESS (COMMAND \"${CMAKE_COMMAND}\" -E copy_directory \"${CMAKE_Fortran_MODULE_DIRECTORY}\" \"${INSTALL_INCLUDE_DIR}/${CMAKE_PROJECT_NAME}\" )" )
