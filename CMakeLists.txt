cmake_minimum_required(VERSION 2.8.12)

if(POLICY CMP0054)
    cmake_policy(SET CMP0054 NEW)
endif()

option(HARFBUZZ_BUILD_ICU "Enable building of ICU" OFF)
if (APPLE)
    option(HB_HAVE_CORETEXT "Enable CoreText shaper backend on macOS" ON)
endif ()

set(SKIP_INSTALL_ALL ON)

## Freetype2 ##
###############

# Stop Freetype from trying to link BZip2. For unclear reasons, BZip2 causes linker errors when targeting iOS simulator.
set(CMAKE_DISABLE_FIND_PACKAGE_BZip2 TRUE)

# Stop Freetype from trying to link Harfbuzz, since we will link our own version.
set(CMAKE_DISABLE_FIND_PACKAGE_HarfBuzz TRUE)

if (IOS)
    # Freetype relies on check_include_file() to detect Unix headers, but some versions of CMake fail to find these files on iOS.
    # We know these files exist on iOS, so just force the variables to be set.
    set(HAVE_UNISTD_H 1 CACHE INTERNAL "Have include unistd.h" FORCE)
    set(HAVE_FCNTL_H 1 CACHE INTERNAL "Have include fcntl.h" FORCE)
    set(HAVE_STDINT_H 1 CACHE INTERNAL "Have include stdint.h" FORCE)
endif()

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/freetype)

## ICU-common ##
################
if (HARFBUZZ_BUILD_ICU)
    add_library(icucommon
        icu/common/appendable.cpp
        icu/common/bmpset.cpp
        icu/common/brkeng.cpp
        icu/common/brkiter.cpp
        icu/common/bytesinkutil.cpp
        icu/common/bytestream.cpp
        icu/common/bytestrie.cpp
        icu/common/bytestriebuilder.cpp
        icu/common/bytestrieiterator.cpp
        icu/common/caniter.cpp
        icu/common/characterproperties.cpp
        icu/common/chariter.cpp
        icu/common/charstr.cpp
        icu/common/cmemory.cpp
        icu/common/cstring.cpp
        icu/common/cwchar.cpp
        icu/common/dictbe.cpp
        icu/common/dictionarydata.cpp
        icu/common/dtintrv.cpp
        icu/common/edits.cpp
        icu/common/errorcode.cpp
        icu/common/filteredbrk.cpp
        icu/common/filterednormalizer2.cpp
        icu/common/icudataver.cpp
        icu/common/icuplug.cpp
        icu/common/loadednormalizer2impl.cpp
        icu/common/localebuilder.cpp
        icu/common/localematcher.cpp
        icu/common/localeprioritylist.cpp
        icu/common/locavailable.cpp
        icu/common/locbased.cpp
        icu/common/locdispnames.cpp
        icu/common/locdistance.cpp
        icu/common/locdspnm.cpp
        icu/common/locid.cpp
        icu/common/loclikely.cpp
        icu/common/loclikelysubtags.cpp
        icu/common/locmap.cpp
        icu/common/locresdata.cpp
        icu/common/locutil.cpp
        icu/common/lsr.cpp
        icu/common/messagepattern.cpp
        icu/common/normalizer2.cpp
        icu/common/normalizer2impl.cpp
        icu/common/normlzr.cpp
        icu/common/parsepos.cpp
        icu/common/patternprops.cpp
        icu/common/pluralmap.cpp
        icu/common/propname.cpp
        icu/common/propsvec.cpp
        icu/common/punycode.cpp
        icu/common/putil.cpp
        icu/common/rbbi.cpp
        icu/common/rbbi_cache.cpp
        icu/common/rbbidata.cpp
        icu/common/rbbinode.cpp
        icu/common/rbbirb.cpp
        icu/common/rbbiscan.cpp
        icu/common/rbbisetb.cpp
        icu/common/rbbistbl.cpp
        icu/common/rbbitblb.cpp
        icu/common/resbund.cpp
        icu/common/resbund_cnv.cpp
        icu/common/resource.cpp
        icu/common/restrace.cpp
        icu/common/ruleiter.cpp
        icu/common/schriter.cpp
        icu/common/serv.cpp
        icu/common/servlk.cpp
        icu/common/servlkf.cpp
        icu/common/servls.cpp
        icu/common/servnotf.cpp
        icu/common/servrbf.cpp
        icu/common/servslkf.cpp
        icu/common/sharedobject.cpp
        icu/common/simpleformatter.cpp
        icu/common/static_unicode_sets.cpp
        icu/common/stringpiece.cpp
        icu/common/stringtriebuilder.cpp
        icu/common/uarrsort.cpp
        icu/common/ubidi.cpp
        icu/common/ubidi_props.cpp
        icu/common/ubidiln.cpp
        icu/common/ubiditransform.cpp
        icu/common/ubidiwrt.cpp
        icu/common/ubrk.cpp
        icu/common/ucase.cpp
        icu/common/ucasemap.cpp
        icu/common/ucasemap_titlecase_brkiter.cpp
        icu/common/ucat.cpp
        icu/common/uchar.cpp
        icu/common/ucharstrie.cpp
        icu/common/ucharstriebuilder.cpp
        icu/common/ucharstrieiterator.cpp
        icu/common/uchriter.cpp
        icu/common/ucln_cmn.cpp
        icu/common/ucmndata.cpp
        icu/common/ucnv.cpp
        icu/common/ucnv2022.cpp
        icu/common/ucnv_bld.cpp
        icu/common/ucnv_cb.cpp
        icu/common/ucnv_cnv.cpp
        icu/common/ucnv_ct.cpp
        icu/common/ucnv_err.cpp
        icu/common/ucnv_ext.cpp
        icu/common/ucnv_io.cpp
        icu/common/ucnv_lmb.cpp
        icu/common/ucnv_set.cpp
        icu/common/ucnv_u16.cpp
        icu/common/ucnv_u32.cpp
        icu/common/ucnv_u7.cpp
        icu/common/ucnv_u8.cpp
        icu/common/ucnvbocu.cpp
        icu/common/ucnvdisp.cpp
        icu/common/ucnvhz.cpp
        icu/common/ucnvisci.cpp
        icu/common/ucnvlat1.cpp
        icu/common/ucnvmbcs.cpp
        icu/common/ucnvscsu.cpp
        icu/common/ucnvsel.cpp
        icu/common/ucol_swp.cpp
        icu/common/ucptrie.cpp
        icu/common/ucurr.cpp
        icu/common/udata.cpp
        icu/common/udatamem.cpp
        icu/common/udataswp.cpp
        icu/common/uenum.cpp
        icu/common/uhash.cpp
        icu/common/uhash_us.cpp
        icu/common/uidna.cpp
        icu/common/uinit.cpp
        icu/common/uinvchar.cpp
        icu/common/uiter.cpp
        icu/common/ulist.cpp
        icu/common/uloc.cpp
        icu/common/uloc_keytype.cpp
        icu/common/uloc_tag.cpp
        icu/common/umapfile.cpp
        icu/common/umath.cpp
        icu/common/umutablecptrie.cpp
        icu/common/umutex.cpp
        icu/common/unames.cpp
        icu/common/unifiedcache.cpp
        icu/common/unifilt.cpp
        icu/common/unifunct.cpp
        icu/common/uniset.cpp
        icu/common/uniset_closure.cpp
        icu/common/uniset_props.cpp
        icu/common/unisetspan.cpp
        icu/common/unistr.cpp
        icu/common/unistr_case.cpp
        icu/common/unistr_case_locale.cpp
        icu/common/unistr_cnv.cpp
        icu/common/unistr_props.cpp
        icu/common/unistr_titlecase_brkiter.cpp
        icu/common/unorm.cpp
        icu/common/unormcmp.cpp
        icu/common/uobject.cpp
        icu/common/uprops.cpp
        icu/common/ures_cnv.cpp
        icu/common/uresbund.cpp
        icu/common/uresdata.cpp
        icu/common/usc_impl.cpp
        icu/common/uscript.cpp
        icu/common/uscript_props.cpp
        icu/common/uset.cpp
        icu/common/uset_props.cpp
        icu/common/usetiter.cpp
        icu/common/ushape.cpp
        icu/common/usprep.cpp
        icu/common/ustack.cpp
        icu/common/ustr_cnv.cpp
        icu/common/ustr_titlecase_brkiter.cpp
        icu/common/ustr_wcs.cpp
        icu/common/ustrcase.cpp
        icu/common/ustrcase_locale.cpp
        icu/common/ustrenum.cpp
        icu/common/ustrfmt.cpp
        icu/common/ustring.cpp
        icu/common/ustrtrns.cpp
        icu/common/utext.cpp
        icu/common/utf_impl.cpp
        icu/common/util.cpp
        icu/common/util_props.cpp
        icu/common/utrace.cpp
        icu/common/utrie.cpp
        icu/common/utrie2.cpp
        icu/common/utrie2_builder.cpp
        icu/common/utrie_swap.cpp
        icu/common/uts46.cpp
        icu/common/utypes.cpp
        icu/common/uvector.cpp
        icu/common/uvectr32.cpp
        icu/common/uvectr64.cpp
        icu/common/wintz.cpp
        icu/stubdata/stubdata.cpp
        )
    target_include_directories(icucommon PUBLIC
        ${CMAKE_CURRENT_SOURCE_DIR}/icu/common
        ${CMAKE_CURRENT_SOURCE_DIR}/icu/stubdata
        ${CMAKE_CURRENT_SOURCE_DIR}/icu/common/unicode)
    target_compile_definitions(icucommon PUBLIC
        -DU_STATIC_IMPLEMENTATION=1
        #-DICU_DATA_DIR_PREFIX_ENV_VAR="ANDROID_ROOT"
        -DPIC
        -DU_COMMON_IMPLEMENTATION)
        #-DU_HAVE_NL_LANGINFO_CODESET=0
        #-DU_TIMEZONE=0
        #-DUCONFIG_NO_TRANSLITERATION=1)
    # icucommon has noexcept usages that are illegal in c++17, so build as c++14 instead.
    set_target_properties(icucommon PROPERTIES CXX_STANDARD 14)
    if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
        target_compile_options(icucommon
          PRIVATE
          -wd4267
          -wd4244)
    endif()
endif()


## Harfbuzz ##
##############

add_library(harfbuzz
    harfbuzz/src/hb-aat-layout.cc
    harfbuzz/src/hb-aat-map.cc
    harfbuzz/src/hb-blob.cc
    harfbuzz/src/hb-buffer-serialize.cc
    harfbuzz/src/hb-buffer.cc
    harfbuzz/src/hb-common.cc
    harfbuzz/src/hb-draw.cc
    harfbuzz/src/hb-face.cc
    harfbuzz/src/hb-fallback-shape.cc
    harfbuzz/src/hb-font.cc
    harfbuzz/src/hb-map.cc
    harfbuzz/src/hb-number.cc
    harfbuzz/src/hb-ot-cff1-table.cc
    harfbuzz/src/hb-ot-cff2-table.cc
    harfbuzz/src/hb-ot-color.cc
    harfbuzz/src/hb-ot-face.cc
    harfbuzz/src/hb-ot-font.cc
    harfbuzz/src/hb-ot-layout.cc
    harfbuzz/src/hb-ot-map.cc
    harfbuzz/src/hb-ot-math.cc
    harfbuzz/src/hb-ot-meta.cc
    harfbuzz/src/hb-ot-metrics.cc
    harfbuzz/src/hb-ot-name.cc
    harfbuzz/src/hb-ot-shape-complex-arabic.cc
    harfbuzz/src/hb-ot-shape-complex-default.cc
    harfbuzz/src/hb-ot-shape-complex-hangul.cc
    harfbuzz/src/hb-ot-shape-complex-hebrew.cc
    harfbuzz/src/hb-ot-shape-complex-indic-table.cc
    harfbuzz/src/hb-ot-shape-complex-indic.cc
    harfbuzz/src/hb-ot-shape-complex-khmer.cc
    harfbuzz/src/hb-ot-shape-complex-myanmar.cc
    harfbuzz/src/hb-ot-shape-complex-thai.cc
    harfbuzz/src/hb-ot-shape-complex-use-table.cc
    harfbuzz/src/hb-ot-shape-complex-use.cc
    harfbuzz/src/hb-ot-shape-complex-vowel-constraints.cc
    harfbuzz/src/hb-ot-shape-fallback.cc
    harfbuzz/src/hb-ot-shape-normalize.cc
    harfbuzz/src/hb-ot-shape.cc
    harfbuzz/src/hb-ot-tag.cc
    harfbuzz/src/hb-ot-var.cc
    harfbuzz/src/hb-set.cc
    harfbuzz/src/hb-shape-plan.cc
    harfbuzz/src/hb-shape.cc
    harfbuzz/src/hb-shaper.cc
    harfbuzz/src/hb-static.cc
    harfbuzz/src/hb-style.cc
    harfbuzz/src/hb-ucd.cc
    harfbuzz/src/hb-unicode.cc
    )

# FreeType integration
target_sources(harfbuzz PRIVATE harfbuzz/src/hb-ft.cc)
target_include_directories(harfbuzz PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/freetype/include)
target_compile_definitions(harfbuzz PRIVATE -DHAVE_FREETYPE)

# ICU integration
if (HARFBUZZ_BUILD_ICU)
    target_sources(harfbuzz PRIVATE harfbuzz/src/hb-icu.cc)
    target_include_directories(harfbuzz PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/icu/common)
    target_link_libraries(harfbuzz PRIVATE icucommon)
    target_compile_definitions(harfbuzz PRIVATE -DHAVE_ICU -DHAVE_ICU_BUILTIN)
    set(HARFBUZZ_UNICODE_LIB icucommon)
endif()

# CoreText integration
if(APPLE AND HB_HAVE_CORETEXT)
    target_compile_definitions(harfbuzz PRIVATE -DHAVE_CORETEXT)
    target_sources(harfbuzz PRIVATE harfbuzz/src/hb-coretext.cc)

    if (IOS)
        # CoreText and CoreGraphics needed for ios
        target_link_libraries(harfbuzz PRIVATE
            "-framework CoreText"
            "-framework CoreGraphics")
    else(IOS)
        # ApplicationServices needed for osx
        target_link_libraries(harfbuzz PRIVATE
            "-framework ApplicationServices")
    endif(IOS)

endif()

# Provide own config file
target_sources(harfbuzz PRIVATE "${CMAKE_CURRENT_SOURCE_DIR}/config.h")
target_compile_definitions(harfbuzz PRIVATE -DHAVE_CONFIG_H)

target_include_directories(harfbuzz
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/harfbuzz/src
    PRIVATE
    ${CMAKE_CURRENT_SOURCE_DIR}/)

option(USE_INTEL_ATOMIC_PRIMITIVES "Platform needs to define HAVE_INTEL_ATOMIC_PRIMITIVES" FALSE)

if(USE_INTEL_ATOMIC_PRIMITIVES)
    add_definitions(-DHAVE_INTEL_ATOMIC_PRIMITIVES)
endif()

set(HARFBUZZ_LIBRARIES
    freetype
    ${HARFBUZZ_UNICODE_LIB}
    CACHE INTERNAL "Harfbuzz libraries" FORCE)

if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
    target_compile_options(harfbuzz PRIVATE -utf-8 -wd4996 -wd4267 -wd4244)
endif()

if (CMAKE_CXX_COMPILER_ID STREQUAL "Clang" OR CMAKE_CXX_COMPILER_ID STREQUAL "GNU")
    # Apply compiler flags from Harfbuzz CMake config.
    target_compile_options(harfbuzz
        PRIVATE
        -fno-rtti
        -fno-exceptions
        -fno-threadsafe-statics
    )
endif()

target_compile_definitions(harfbuzz PUBLIC
    PRIVATE
    -DHAVE_OT # Include the default "ot" shaper
    -DHB_NO_MT # Exclude multi-threading support
    )

set_target_properties(harfbuzz PROPERTIES CXX_STANDARD 11)
