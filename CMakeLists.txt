cmake_minimum_required(VERSION 3.2 FATAL_ERROR)
project(SciDAVis VERSION 1.0 LANGUAGES CXX C)

# Specify options for build
option(BUILD_STATIC_LIBS "Build static libraries" ON)
option(BUILD_DYNAMIC_LIBS "Build dynamic libraries" ON)
option(BUILD_TESTS "Build tests" OFF)
option(FIT_PLUGINS "Build fit plugins" ON)
option(PYTHON_SCRIPTING "Enable Python scripting support" ON)


# Find includes in corresponding build directories
set(CMAKE_INCLUDE_CURRENT_DIR ON)
# Instruct CMake to run moc automatically when needed.
set(CMAKE_AUTOMOC ON)

# Find the QtWidgets library
find_package(Qt5Widgets)

# Tell CMake to create the helloworld executable
add_executable(scidavis WIN32 main.cpp)

# Subdirs
add_subdirectory(3rd_party)
add_subdirectory(libscidavis)
add_subdirectory(scidavis)

if(BUILD_TESTS)
    add_subdirectory(test)
endif(BUILD_TESTS)

if(FIT_PLUGINS)
    # optional fit plugins
    # mxe is a static build, so these are included elsewhere
    add_subdirectory(fitPlugins)
endif(FIT_PLUGINS)

# Translations
set(TRANSLATIONS "")
foreach(LANG de es nl fr ru ja sv pt-br cs cs-alt)
    set(TRANSLATIONS "${TRANSLATIONS} %{CMAKE_CURRENT_SOURCE_DIR}/scidavis/translations/scidavis_${LANG}.ts")
endforeach()
# Use the Widgets module from Qt 5.
target_link_libraries(scidavis Qt5::Widgets)

###############################################################################
############################### packaging #####################################
###############################################################################

# all install commands get the same destination. this allows us to use paths
# relative to the executable.
install(TARGETS example DESTINATION example_destination)
# this is basically a repeat of the file copy instruction that copies the
# resources in the build directory, but here we tell cmake that we want it
# in the package
install(DIRECTORY resources DESTINATION example_destination)

# now comes everything we need, to create a package
# there are a lot more variables you can set, and some
# you need to set for some package types, but we want to
# be minimal here
set(CPACK_PACKAGE_NAME "SciDAVis")
set(CPACK_PACKAGE_VERSION "1.D013")

# we don't want to split our program up into several things
set(CPACK_MONOLITHIC_INSTALL 1)

# This must be last
include(CPack)