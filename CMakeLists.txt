# This CMakeLists.txt is only a ESP-IDF component declaration
include(FetchContent)
set(LIBLIGHTMODBUS_REPO_DIR "${CMAKE_CURRENT_LIST_DIR}/liblightmodbus")

# Fetch liblightmodbus source code from GitHub
FetchContent_Populate(liblightmodbus-src
	GIT_REPOSITORY "https://github.com/jacajack/liblightmodbus.git"
	GIT_TAG "dev-v3.0"
	SOURCE_DIR   "${LIBLIGHTMODBUS_REPO_DIR}"
)

# Copy config file to liblightmodbus include directory
configure_file(
	"${CMAKE_CURRENT_LIST_DIR}/src/esp.config.h"
	"${LIBLIGHTMODBUS_REPO_DIR}/include/lightmodbus/esp.config.h"
	COPYONLY
)

# Register component
idf_component_register(
	SRCS "src/impl.c"
	INCLUDE_DIRS "${LIBLIGHTMODBUS_REPO_DIR}/include"
)

# Inform liblightmodbus to use ESP config header
idf_build_set_property(COMPILE_DEFINITIONS "-DLIGHTMODBUS_USE_CONFIG_FILE" APPEND)
idf_build_set_property(COMPILE_DEFINITIONS "-DLIGHTMODBUS_CONFIG_FILE=\"esp.config.h\"" APPEND)
