# CMake file for strawberry-ini
# A simple ini parser, reader, dictionary
# ======================================================================

cmake_minimum_required( VERSION 2.6.0 )
set(CMAKE_LEGACY_CYGWIN_WIN32 0)

project(strawberry-ini)
set(STRAWBERRY_VERSION "1.0" )

set(CMAKE_ARCHIVE_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/lib)
set(CMAKE_LIBRARY_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/lib)
set(CMAKE_RUNTIME_OUTPUT_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}/lib)

# list of sources for the strawberry-ini library
set(strawberry_ini_sources
            ini_parser.c ini_parser.h
            ini_dictionary.c ini_dictionary.h
            ini_reader.c ini_reader.h
    )

# static version of library
add_library( strawberryIniStatic STATIC ${strawberry_ini_sources} )
set_target_properties( strawberryIniStatic PROPERTIES OUTPUT_NAME "strawberryIni" )
# shared version of library
add_library( strawberryIni SHARED ${strawberry_ini_sources} )
set_target_properties( strawberryIni PROPERTIES SOVERSION ${STRAWBERRY_VERSION} )

# process subdirectories
include_directories(${CMAKE_CURRENT_SOURCE_DIR})
add_subdirectory(examples)

if(STRAWBERRY_INI_BUILD_TESTS)
    enable_testing()
    add_subdirectory(tests)
endif(STRAWBERRY_INI_BUILD_TESTS)

# add a target to generate API documentation with Doxygen
find_package(Doxygen QUIET)
if(DOXYGEN_FOUND)
    message(STATUS "Doxygen found. Adding 'doc' target.")
    configure_file(${CMAKE_CURRENT_SOURCE_DIR}/Doxyfile.in ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile @ONLY)
    add_custom_target(doc
      ${DOXYGEN_EXECUTABLE} ${CMAKE_CURRENT_BINARY_DIR}/Doxyfile
      WORKING_DIRECTORY ${CMAKE_CURRENT_BINARY_DIR}
      COMMENT "Generating API documentation with Doxygen" VERBATIM
)
endif(DOXYGEN_FOUND)
