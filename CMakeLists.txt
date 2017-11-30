project (com.github.tobus3000.mqtt-warlock)

cmake_minimum_required (VERSION 2.6)
list (APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake)
list (APPEND CMAKE_MODULE_PATH ${CMAKE_SOURCE_DIR}/cmake/src)

# install data directory
set (DATADIR "${CMAKE_INSTALL_PREFIX}/share")
set (PKGDATADIR "${DATADIR}/com.github.tobus3000.mqtt-warlock")

set (EXEC_NAME "com.github.tobus3000.mqtt-warlock")
set (RELEASE_NAME "MQTT Warlock")
set (VERSION "0.1")
set (VERSION_INFO "work in progress....")

# Translation files
set (GETTEXT_PACKAGE "${CMAKE_PROJECT_NAME}")
add_definitions (-DGETTEXT_PACKAGE=\"${GETTEXT_PACKAGE}\")
add_subdirectory (po)

# pkgconfig & dependencies
find_package(PkgConfig)
pkg_check_modules(DEPS REQUIRED gtk+-3.0)
add_definitions(${DEPS_CFLAGS})
link_libraries(${DEPS_LIBRARIES})
link_directories(${DEPS_LIBRARY_DIRS})
find_package(Vala REQUIRED)
include(ValaVersion)
ensure_vala_version("0.16" MINIMUM)

# files we want to compile
include(ValaPrecompile)
vala_precompile(VALA_C ${EXEC_NAME}
    src/mqtt-warlock.vala

PACKAGES
    gtk+-3.0
)

# executable name
add_executable(${EXEC_NAME} ${VALA_C})

# install binaries
install (TARGETS ${EXEC_NAME} RUNTIME DESTINATION bin)

# install .desktop file
install (FILES ${CMAKE_CURRENT_SOURCE_DIR}/data/com.github.tobus3000.mqtt-warlock.desktop DESTINATION ${DATADIR}/applications/)

# install .appdata.xml file for AppCenter
install (FILES ${CMAKE_CURRENT_SOURCE_DIR}/data/com.github.tobus3000.mqtt-warlock.appdata.xml DESTINATION ${DATADIR}/metainfo/)
