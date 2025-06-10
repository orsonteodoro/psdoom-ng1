set(CMAKE_MODULE_PATH "${CMAKE_CURRENT_SOURCE_DIR}/cmake")

cmake_minimum_required(VERSION 3.7.2)
include(GNUInstallDirs)
project("psdoom-ng" VERSION 3.1.0 LANGUAGES C) # 2025.06.08.3.1.0

option(BUILD_ALL_GAMES "Build all games" OFF)

set(CMAKE_C_STANDARD 99)

# Autotools variables
set(top_srcdir ${CMAKE_CURRENT_SOURCE_DIR})
set(top_builddir ${CMAKE_CURRENT_BINARY_DIR})

# AC_INIT variables
set(PACKAGE_NAME "${PROJECT_NAME}")
set(PACKAGE_TARNAME "psdoom-ng")
set(PACKAGE_VERSION "${PROJECT_VERSION}")
set(PACKAGE_STRING "${PROJECT_NAME} ${PROJECT_VERSION}")
set(PACKAGE_BUGREPORT "mod: https://github.com/orsonteodoro/psdoom-ng1, engine: chocolate-doom-dev-list@chocolate-doom.org")
set(PACKAGE_URL "mod: https://github.com/orsonteodoro/psdoom-ng, engine: https://www.chocolate-doom.org/")
set(PACKAGE_MAINTAINER "mod: Orson Teodoro, engine: Simon Howard")

string(REGEX REPLACE " Doom$" "" PACKAGE_SHORTNAME "${PACKAGE_NAME}")
set(PACKAGE_COPYRIGHT "Copyright (C) 1993-2024")
set(PACKAGE_LICENSE "GNU General Public License, version 2")

# Any settings that should apply to all targets in this directory and all
# subdirectories should go here.  Use judiciously.
if(MSVC)
    add_definitions("/D_CRT_SECURE_NO_WARNINGS" "/D_CRT_SECURE_NO_DEPRECATE"
                    "/D_CRT_NONSTDC_NO_DEPRECATE")
else()
    add_compile_options("-Wall" "-Wdeclaration-after-statement"
                        "-Wredundant-decls")
endif()

option(CMAKE_FIND_PACKAGE_PREFER_CONFIG
       "Lookup package config files before using find modules" On)

option(ENABLE_SDL2_NET "Enable SDL2_net" On)
option(ENABLE_SDL2_MIXER "Enable SDL2_mixer" On)

if (NOT DEFINED EMSCRIPTEN)
    find_package(SDL2 2.0.14)
endif()
if(ENABLE_SDL2_MIXER)
    if (NOT DEFINED EMSCRIPTEN)
        find_package(SDL2_mixer 2.0.2)
    endif()
else()
    add_compile_definitions(DISABLE_SDL2MIXER=1)
endif()

if(ENABLE_SDL2_NET)
    if (NOT DEFINED EMSCRIPTEN)
        find_package(SDL2_net 2.0.0)
    endif()
else()
    add_compile_definitions(DISABLE_SDL2NET=1)
endif()


# Check for libsamplerate.
find_package(SampleRate)
if(SampleRate_FOUND)
    set(HAVE_LIBSAMPLERATE TRUE)
endif()

# Check for libpng.
find_package(PNG)
if(PNG_FOUND)
    set(HAVE_LIBPNG TRUE)
endif()

# Check for FluidSynth.
find_package(FluidSynth 2.2.0)
if(FluidSynth_FOUND AND ENABLE_SDL2_MIXER)
    set(HAVE_FLUIDSYNTH TRUE)
endif()

find_package(m)

include(CheckSymbolExists)
include(CheckIncludeFile)
check_symbol_exists(strcasecmp "strings.h" HAVE_DECL_STRCASECMP)
check_symbol_exists(strncasecmp "strings.h" HAVE_DECL_STRNCASECMP)
check_include_file("dirent.h" HAVE_DIRENT_H)

string(CONCAT WINDOWS_RC_VERSION "${PROJECT_VERSION_MAJOR}, "
    "${PROJECT_VERSION_MINOR}, ${PROJECT_VERSION_PATCH}, 0")

# Without a hyphen. This is used for the bash-completion scripts.
string(TOLOWER "${PACKAGE_SHORTNAME}" PROGRAM_SPREFIX)

# With a hyphen, used almost everywhere else.
set(PROGRAM_PREFIX "${PROGRAM_SPREFIX}-")

configure_file(cmake/config.h.cin config.h)

configure_file(src/doom-res.rc.in src/doom-res.rc)
configure_file(src/heretic-res.rc.in src/heretic-res.rc)
configure_file(src/hexen-res.rc.in src/hexen-res.rc)
configure_file(src/setup-res.rc.in src/setup-res.rc)
configure_file(src/setup/setup-manifest.xml.in src/setup/setup-manifest.xml)
configure_file(src/strife-res.rc.in src/strife-res.rc)
configure_file(src/psdoom-ng.appdata.xml.in src/psdoom-ng.appdata.xml)
configure_file(src/Heretic.metainfo.xml.in src/Heretic.metainfo.xml)
configure_file(src/Doom.metainfo.xml.in src/Doom.metainfo.xml)
configure_file(src/Hexen.metainfo.xml.in src/Hexen.metainfo.xml)
configure_file(src/Strife.metainfo.xml.in src/Strife.metainfo.xml)
configure_file(src/setup/setup-manifest.xml.in ./src/setup/setup-manifest.xml)
configure_file(src/Doom.desktop.in src/${PROGRAM_PREFIX}Doom.desktop)
configure_file(src/psdoom-ng.desktop.in src/psdoom-ng.desktop)
configure_file(src/Heretic.desktop.in src/${PROGRAM_PREFIX}Heretic.desktop)
configure_file(src/Strife.desktop.in src/${PROGRAM_PREFIX}Strife.desktop)
configure_file(src/Doom_Screensaver.desktop.in src/${PROGRAM_PREFIX}Doom_Screensaver.desktop)
configure_file(src/setup/Setup.desktop.in src/setup/${PROGRAM_PREFIX}Setup.desktop)
configure_file(src/Hexen.desktop.in src/${PROGRAM_PREFIX}Hexen.desktop)

install(FILES "${CMAKE_SOURCE_DIR}/psdoom-ng.conf.example" DESTINATION "${CMAKE_INSTALL_DATADIR}/psdoom-ng")
install(FILES "${CMAKE_BINARY_DIR}/src/psdoom-ng.appdata.xml" DESTINATION "${CMAKE_INSTALL_DATADIR}/psdoom-ng")
install(FILES "${CMAKE_BINARY_DIR}/src/psdoom-ng.appdata.xml" DESTINATION "${CMAKE_INSTALL_DATADIR}/metainfo")
install(FILES "${CMAKE_SOURCE_DIR}/man/psdoom-ng.1" DESTINATION "${CMAKE_INSTALL_MANDIR}/man1")
install(FILES "${CMAKE_SOURCE_DIR}/psd.in" DESTINATION "bin" RENAME "psd" PERMISSIONS OWNER_READ OWNER_WRITE OWNER_EXECUTE GROUP_READ GROUP_EXECUTE WORLD_READ WORLD_EXECUTE)
if(BUILD_ALL_GAMES)
    install(FILES "${CMAKE_BINARY_DIR}/src/${PROGRAM_PREFIX}Doom.desktop" DESTINATION "${CMAKE_INSTALL_DATADIR}/applications")
    install(FILES "${CMAKE_BINARY_DIR}/src/${PROGRAM_PREFIX}Heretic.desktop" DESTINATION "${CMAKE_INSTALL_DATADIR}/applications")
    install(FILES "${CMAKE_BINARY_DIR}/src/${PROGRAM_PREFIX}Strife.desktop" DESTINATION "${CMAKE_INSTALL_DATADIR}/applications")
    install(FILES "${CMAKE_BINARY_DIR}/src/${PROGRAM_PREFIX}Doom_Screensaver.desktop" DESTINATION "${CMAKE_INSTALL_DATADIR}/applications")
    install(FILES "${CMAKE_BINARY_DIR}/src/${PROGRAM_PREFIX}Hexen.desktop" DESTINATION "${CMAKE_INSTALL_DATADIR}/applications")
endif()
install(FILES "${CMAKE_BINARY_DIR}/src/setup/${PROGRAM_PREFIX}Setup.desktop" DESTINATION "${CMAKE_INSTALL_DATADIR}/applications")
install(FILES "${CMAKE_BINARY_DIR}/src/psdoom-ng.desktop" DESTINATION "${CMAKE_INSTALL_DATADIR}/applications")

foreach(SUBDIR textscreen opl pcsound src)
    add_subdirectory("${SUBDIR}")
endforeach()
