cmake_minimum_required(VERSION 2.8.12)

if(POLICY CMP0054)
    cmake_policy(SET CMP0054 NEW)
endif()

option(HARFBUZZ_BUILD_ICU "Enable building of ICU" ON)
option(HARFBUZZ_BUILD_UCDN "Enable building of UCDN" OFF)
if (APPLE)
    option(HB_HAVE_CORETEXT "Enable CoreText shaper backend on macOS" ON)
endif ()

## Freetype2 ##
###############
option(WITH_ZLIB "Freetype: Enable zlib " OFF)
option(WITH_BZip2 "Freetype: Enable bzip2 " OFF)
option(WITH_HarfBuzz "Freetype: Enable harfbuzz" OFF)
option(WITH_PNG "Freetype: Build with PNG" OFF)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/freetype)

## ICU-common ##
################
file(GLOB ICUCOMMON_SRC_FILES
    "icu/icu4c/source/common/*.h"
    "icu/icu4c/source/common/*.hh"
    "icu/icu4c/source/common/*.hxx"
    "icu/icu4c/source/common/*.hpp"
    "icu/icu4c/source/common/*.c"
    "icu/icu4c/source/common/*.cc"
    "icu/icu4c/source/common/*.cxx"
    "icu/icu4c/source/common/*.cpp"
)
if (HARFBUZZ_BUILD_ICU)
    add_library(
        icucommon
        ${ICUCOMMON_SRC_FILES}
    )
    target_include_directories(
        icucommon PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/icu/icu4c/source/common
        ${CMAKE_CURRENT_SOURCE_DIR}/icu/icu4c/source/stubdata
        ${CMAKE_CURRENT_SOURCE_DIR}/icu/icu4c/source/common/unicode
    )
    target_compile_definitions(
        icucommon PUBLIC
        -DU_STATIC_IMPLEMENTATION=1
        #-DICU_DATA_DIR_PREFIX_ENV_VAR="ANDROID_ROOT"
        -DPIC
        -DU_COMMON_IMPLEMENTATION
        #-DU_HAVE_NL_LANGINFO_CODESET=0
        #-DU_TIMEZONE=0
        #-DUCONFIG_NO_TRANSLITERATION=1
    )
    if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
        target_compile_options(
            icucommon
            PRIVATE
            -wd4267
            -wd4244
        )
    endif()
endif()

if (HARFBUZZ_BUILD_ICU)
    set(HARFBUZZ_UNICODE_SRC harfbuzz/src/hb-ucdn.cc)
    set(HARFBUZZ_UNICODE_INCLUDE ${CMAKE_CURRENT_SOURCE_DIR}/icu/common)
    set(HARFBUZZ_UNICODE_LIB icucommon)
    set(HARFBUZZ_UNICODE_DEFINE -DHAVE_ICU -DHAVE_ICU_BUILTIN)
endif()

if (HARFBUZZ_BUILD_UCDN)
    set(HARFBUZZ_UNICODE_SRC harfbuzz/src/hb-ft.cc)
    set(HARFBUZZ_UNICODE_INCLUDE ${CMAKE_CURRENT_SOURCE_DIR}/harfbuzz/src/hb-ucdn/)
    set(HARFBUZZ_UNICODE_DEFINE -DHAVE_UCDN)
endif()

## Harfbuzz ##
##############
file(GLOB HARFBUZZ_SRC_FILES
    "harfbuzz/src/*.h"
    "harfbuzz/src/*.hh"
    "harfbuzz/src/*.hxx"
    "harfbuzz/src/*.hpp"
    "harfbuzz/src/*.c"
    "harfbuzz/src/*.cc"
    "harfbuzz/src/*.cxx"
    "harfbuzz/src/*.cpp"
)

if(APPLE AND HB_HAVE_CORETEXT)
    # Apple Advanced Typography
    add_definitions(-DHAVE_CORETEXT)

    list(APPEND HARFBUZZ_SRC_FILES harfbuzz/src/hb-coretext.cc)

    if (IOS)
        # CORE_TEXT needed for ios
        find_library(CORE_TEXT CoreText)
        if (CORE_TEXT)
            list(APPEND THIRD_PARTY_LIBS ${CORE_TEXT})
        endif (CORE_TEXT)

        # CORE_GRAPHICS needed for ios
        find_library(CORE_GRAPHICS CoreGraphics)
        if (CORE_GRAPHICS)
            list(APPEND THIRD_PARTY_LIBS ${CORE_GRAPHICS})
        endif (CORE_GRAPHICS)
    else(IOS)
        # APPLICATION_SERVICES_FRAMEWORK needed for osx
        find_library(APPLICATION_SERVICES_FRAMEWORK ApplicationServices)
        if (APPLICATION_SERVICES_FRAMEWORK)
            list(APPEND THIRD_PARTY_LIBS ${APPLICATION_SERVICES_FRAMEWORK})
        endif (APPLICATION_SERVICES_FRAMEWORK)
    endif(IOS)

endif()

list(APPEND HARFBUZZ_SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/config.h")
add_library(harfbuzz ${HARFBUZZ_SRC_FILES})
target_link_libraries(harfbuzz ${THIRD_PARTY_LIBS})

if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    set_source_files_properties(
        ${HARFBUZZ_SRC_FILES}
        PROPERTIES
        COMPILE_FLAGS  "-utf-8 -wd4996 -wd4267 -wd4244"
    )
endif()

set(
    HARFBUZZ_INCLUDE_DIRS
    ${CMAKE_CURRENT_SOURCE_DIR}/harfbuzz/src
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/include/
    ${HARFBUZZ_UNICODE_INCLUDE}
    ${CMAKE_CURRENT_SOURCE_DIR}/
    CACHE INTERNAL "Harfbuzz include dirs" FORCE
)

target_include_directories(
    harfbuzz
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/harfbuzz-generated
    ${CMAKE_CURRENT_SOURCE_DIR}/harfbuzz/src
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/include
    ${HARFBUZZ_UNICODE_INCLUDE}
    PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/
)

option(USE_INTEL_ATOMIC_PRIMITIVES "Platform needs to define HAVE_INTEL_ATOMIC_PRIMITIVES" FALSE)

if(USE_INTEL_ATOMIC_PRIMITIVES)
    add_definitions(-DHAVE_INTEL_ATOMIC_PRIMITIVES)
endif()

set(
    HARFBUZZ_LIBRARIES
    freetype
    ${HARFBUZZ_UNICODE_LIB}
    CACHE INTERNAL "Harfbuzz libraries" FORCE
)

if (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
    target_compile_options(
        harfbuzz
        PRIVATE
        -Wno-vla-extension
        -Wno-extended-offsetof
    )
endif()

target_compile_definitions(
    harfbuzz PUBLIC
    PRIVATE
    -DHAVE_OT
    # Provide own config file
    -DHAVE_CONFIG_H
    ${HARFBUZZ_UNICODE_DEFINE}
    ${HARFBUZZ_CORETEXT_DEFINE}
    -DHB_NO_MT
)

