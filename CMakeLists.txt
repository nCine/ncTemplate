set(NCPROJECT_NAME "ncTemplate")
set(NCPROJECT_EXE_NAME "nctemplate")
set(NCPROJECT_VENDOR "Angelo Theodorou")
set(NCPROJECT_COPYRIGHT "Copyright ©2019-2021 ${NCPROJECT_VENDOR}")
set(NCPROJECT_DESCRIPTION "A template project for applications and games made with the nCine")
set(NCPROJECT_HOMEPAGE "https://ncine.github.io")
set(NCPROJECT_REVERSE_DNS "io.github.ncine.nctemplate")

set(NCPROJECT_INCLUDE_DIRS include)

set(NCPROJECT_SOURCES
	include/main.h
	src/main.cpp
)

# Don't edit beyond this line
if(CMAKE_SYSTEM_NAME STREQUAL "Android")
	return()
endif()

cmake_minimum_required(VERSION 3.13)
project(${NCPROJECT_NAME})

find_path(NCPROJECT_DIR cmake/project_main.cmake
	PATHS ${nCine_DIR} $ENV{NCINE_DIR}
	PATH_SUFFIXES project ../project ../../../project ../../../share/ncine/project
	NO_CMAKE_FIND_ROOT_PATH # For Emscripten
	DOC "Path to the nCine template project scripts")

if(IS_DIRECTORY ${NCPROJECT_DIR})
	include(${NCPROJECT_DIR}/cmake/project_main.cmake)
else()
	message(FATAL_ERROR "Directory \"${NCPROJECT_DIR}\" does not exist")
endif()
