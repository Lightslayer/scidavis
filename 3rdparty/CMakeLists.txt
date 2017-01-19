#This script is used to painlessly pull in all the third-party dependencies

#https://github.com/boostorg/variant
#https://github.com/beltoforion/muparser
#https://github.com/python/cpython
#https://github.com/sintegrial/qwtplot3d
#https://qwt.svn.sourceforge.net/svnroot/qwt/trunk
#https://github.com/sfalexrog/python-xcompile

cmake_minimum_required(VERSION 2.8)
include(ExternalProject)

if (!USE_QT4)  # prefer Qt5
FIND_PACKAGE(Qt5 REQUIRED)
INCLUDE(${QT_USE_FILE})
else()
FIND_PACKAGE(Qt4 REQUIRED)
INCLUDE(${QT_USE_FILE})
endif()

ExternalProject_Add(CloneBoostVariant
    PREFIX 3rd_party
    #--Download step--------------
    GIT_REPOSITORY https://github.com/boostorg/variant
    GIT_TAG master
    #--Configure step-------------
    CONFIGURE_COMMAND ""
    #--Build step-----------------
    BUILD_COMMAND ""
    #--Install step---------------
    UPDATE_COMMAND "" # Skip annoying updates for every build
    INSTALL_COMMAND ""
)

SET(BOOST_VARIANT_INCLUDE_DIR ${CMAKE_CURRENT_BINARY_DIR}/3rd_party/src/BoostVariant/include)

ExternalProject_Add(CloneMuParser
    PREFIX 3rd_party
    #--Download step--------------
    GIT_REPOSITORY https://github.com/beltoforion/muparser
    GIT_TAG master
    #--Configure step-------------
    CONFIGURE_COMMAND ""
    #--Build step-----------------
    BUILD_COMMAND ""
    #--Install step---------------
    UPDATE_COMMAND "" # Skip annoying updates for every build
    INSTALL_COMMAND ""
)

SET(MUPARSER_INCLUDE_DIR ${CMAKE_CURRENT_BINARY_DIR}/3rd_party/src/MuParser/include)
SET(MUPARSER_LIB_DIR ${CMAKE_CURRENT_BINARY_DIR}/3rd_party/src/MuParser/lib)

add_library(MuParser SHARED IMPORTED)
set_target_properties(MuParser PROPERTIES IMPORTED_LOCATION ${MUPARSER_LIB_DIR}/libMuParser.so)

ExternalProject_Add(CloneQwt
    PREFIX 3rd_party
    #--Download step--------------
    #SVN_REPOSITORY https://qwt.svn.sourceforge.net/svnroot/qwt/trunk/qwt
	GIT_REPOSITORY https://github.com/osakared/qwt
	GIT_TAG master
	#URL http://fxcodebase.com/bin/forexconnect/1.3.1/ForexConnectAPI-1.3.1-Linux-x86_64.tar.gz
    #URL_HASH SHA1=7fdb90a2d45085feb8b76167cae419ad4c211d6b
    #--Configure step-------------
    CONFIGURE_COMMAND ""
    #--Build step-----------------
    BUILD_COMMAND ""
    #--Install step---------------
    UPDATE_COMMAND "" # Skip annoying updates for every build
    INSTALL_COMMAND ""
)

SET(QWT_INCLUDE_DIR ${CMAKE_CURRENT_BINARY_DIR}/3rd_party/src/qwt/include)
SET(QWT_LIB_DIR ${CMAKE_CURRENT_BINARY_DIR}/3rd_party/src/qwt/lib)

add_library(Qwt SHARED IMPORTED)
set_target_properties(Qwt PROPERTIES IMPORTED_LOCATION ${QWT_LIB_DIR}/libQwt.so)


ExternalProject_Add(CloneQwtPlot3d
    PREFIX 3rd_party
    #--Download step--------------
    GIT_REPOSITORY https://github.com/sintegrial/qwtplot3d
    GIT_TAG master
    #--Configure step-------------
    CONFIGURE_COMMAND ""
    #--Build step-----------------
    BUILD_COMMAND ""
    #--Install step---------------
    UPDATE_COMMAND "" # Skip annoying updates for every build
    INSTALL_COMMAND ""
)

ExternalProject_Add(CloneCPython
    PREFIX 3rd_party
    #--Download step--------------
    GIT_REPOSITORY https://github.com/python/cpython
    GIT_TAG master
    #--Configure step-------------
    CONFIGURE_COMMAND ""
    #--Build step-----------------
    BUILD_COMMAND ""
    #--Install step---------------
    UPDATE_COMMAND "" # Skip annoying updates for every build
    INSTALL_COMMAND ""
)

SET(CPYTHON_INCLUDE_DIR ${CMAKE_CURRENT_BINARY_DIR}/3rd_party/src/CPython/include)
SET(FXCM_LIB_DIR ${CMAKE_CURRENT_BINARY_DIR}/3rd_party/src/CPython/lib)

add_library(CPython SHARED IMPORTED)
set_target_properties(CPython PROPERTIES IMPORTED_LOCATION ${CPYTHON_LIB_DIR}/libCPython.so)