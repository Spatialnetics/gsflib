
cmake_minimum_required(VERSION 3.15)

project(GSFLib
    VERSION 3.11
    LANGUAGES C
)

# Build a shared library by default, for ease of compatibility with the LGPL.
option(BUILD_SHARED_LIBS "Build using shared libraries" ON)


# Build the gsflib library
add_subdirectory(source)


# Make the library available under the fancy, modern name GSF::GSFLib
add_library(GSF::GSFLib ALIAS gsflib)
set_property(TARGET gsflib PROPERTY EXPORT_NAME GSFLib)


# Set the appropriate include directory for use both pre- and post-installation
include(GNUInstallDirs)
target_include_directories(gsflib INTERFACE
    "$<BUILD_INTERFACE:${CMAKE_CURRENT_SOURCE_DIR}/source>"
    "$<INSTALL_INTERFACE:${CMAKE_INSTALL_INCLUDEDIR}>"
)

# Install the public header
install(FILES
    source/gsf/gsf.h
    DESTINATION ${CMAKE_INSTALL_INCLUDEDIR}/gsf
)

# Install and export the target
install(TARGETS gsflib EXPORT GSFTargets)
install(EXPORT GSFTargets
    FILE GSFTargets.cmake
    NAMESPACE GSF::
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/GSF
)

# Generate and install the config files that let CMake find the library
include(CMakePackageConfigHelpers)
configure_package_config_file(
    ${CMAKE_CURRENT_SOURCE_DIR}/Config.cmake.in
    "${CMAKE_CURRENT_BINARY_DIR}/GSFConfig.cmake"
    INSTALL_DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/GSF
)
write_basic_package_version_file(
    ${CMAKE_CURRENT_BINARY_DIR}/GSFConfigVersion.cmake
    COMPATIBILITY SameMajorVersion
)
install(FILES
    "${CMAKE_CURRENT_BINARY_DIR}/GSFConfig.cmake"
    "${CMAKE_CURRENT_BINARY_DIR}/GSFConfigVersion.cmake"
    DESTINATION ${CMAKE_INSTALL_LIBDIR}/cmake/GSF
)
