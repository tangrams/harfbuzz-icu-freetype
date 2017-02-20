cmake_minimum_required(VERSION 2.8.12)

if(POLICY CMP0054)
    cmake_policy(SET CMP0054 NEW)
endif()

option(HARFBUZZ_BUILD_ICU "Enable building of ICU" OFF)
option(HARFBUZZ_BUILD_UCDN "Enable building of UCDN" ON)

## Freetype2 ##
###############
option(WITH_ZLIB "Freetype: Enable zlib " OFF)
option(WITH_BZip2 "Freetype: Enable bzip2 " OFF)
option(WITH_HarfBuzz "Freetype: Enable harfbuzz" OFF)
option(WITH_PNG "Freetype: Build with PNG" OFF)

add_subdirectory(${CMAKE_CURRENT_SOURCE_DIR}/freetype)

## ICU-common ##
################
if (HARFBUZZ_BUILD_ICU)
    add_library(icucommon
        icu/common/appendable.cpp
        icu/common/cmemory.c
        icu/common/cstring.c
        icu/common/cwchar.c
        icu/common/icudataver.c
        icu/common/icuplug.c
        icu/common/locmap.c
        icu/common/propsvec.c
        icu/common/uarrsort.c
        icu/common/ubidi.c
        icu/common/ubidi_props.c
        icu/common/ubidiln.c
        icu/common/ubidiwrt.c
        icu/common/ucat.c
        icu/common/uchar.c
        icu/common/ucln_cmn.c
        icu/common/ucmndata.c
        icu/common/ucnv.c
        icu/common/ucnv_cb.c
        icu/common/ucnv_cnv.c
        icu/common/ucnv_ct.c
        icu/common/ucnv_err.c
        icu/common/ucnv_lmb.c
        icu/common/ucnv_set.c
        icu/common/ucnv_u16.c
        icu/common/ucnv_u32.c
        icu/common/ucnv_u7.c
        icu/common/ucnv_u8.c
        icu/common/ucnvdisp.c
        icu/common/ucnvhz.c
        icu/common/ucnvisci.c
        icu/common/ucnvlat1.c
        icu/common/ucnvmbcs.c
        icu/common/ucnvscsu.c
        icu/common/udatamem.c
        icu/common/udataswp.c
        icu/common/uenum.c
        icu/common/uhash.c
        icu/common/uinvchar.c
        icu/common/ulist.c
        icu/common/uloc_tag.c
        icu/common/umapfile.c
        icu/common/umath.c
        icu/common/unorm_it.c
        icu/common/ures_cnv.c
        icu/common/uresdata.c
        icu/common/usc_impl.c
        icu/common/uscript.c
        icu/common/ustr_cnv.c
        icu/common/ustrfmt.c
        icu/common/utf_impl.c
        icu/common/utrace.c
        icu/common/utypes.c
        icu/common/wintz.c
        icu/common/bmpset.cpp
        icu/common/brkeng.cpp
        icu/common/brkiter.cpp
        icu/common/bytestream.cpp
        icu/common/bytestrie.cpp
        icu/common/bytestriebuilder.cpp
        icu/common/bytestrieiterator.cpp
        icu/common/caniter.cpp
        icu/common/chariter.cpp
        icu/common/charstr.cpp
        icu/common/dictbe.cpp
        icu/common/dictionarydata.cpp
        icu/common/dtintrv.cpp
        icu/common/errorcode.cpp
        icu/common/filterednormalizer2.cpp
        icu/common/listformatter.cpp
        icu/common/locavailable.cpp
        icu/common/locbased.cpp
        icu/common/locdispnames.cpp
        icu/common/locid.cpp
        icu/common/loclikely.cpp
        icu/common/locresdata.cpp
        icu/common/locutil.cpp
        icu/common/messagepattern.cpp
        icu/common/normalizer2.cpp
        icu/common/normalizer2impl.cpp
        icu/common/normlzr.cpp
        icu/common/parsepos.cpp
        icu/common/patternprops.cpp
        icu/common/propname.cpp
        icu/common/punycode.cpp
        icu/common/putil.cpp
        icu/common/rbbi.cpp
        icu/common/rbbidata.cpp
        icu/common/rbbinode.cpp
        icu/common/rbbirb.cpp
        icu/common/rbbiscan.cpp
        icu/common/rbbisetb.cpp
        icu/common/rbbistbl.cpp
        icu/common/rbbitblb.cpp
        icu/common/resbund.cpp
        icu/common/resbund_cnv.cpp
        icu/common/ruleiter.cpp
        icu/common/schriter.cpp
        icu/common/serv.cpp
        icu/common/servlk.cpp
        icu/common/servlkf.cpp
        icu/common/servls.cpp
        icu/common/servnotf.cpp
        icu/common/servrbf.cpp
        icu/common/servslkf.cpp
        icu/common/stringpiece.cpp
        icu/common/stringtriebuilder.cpp
        icu/common/ubrk.cpp
        icu/common/ucase.cpp
        icu/common/ucasemap.cpp
        icu/common/ucasemap_titlecase_brkiter.cpp
        icu/common/ucharstrie.cpp
        icu/common/ucharstriebuilder.cpp
        icu/common/ucharstrieiterator.cpp
        icu/common/uchriter.cpp
        icu/common/ucnv2022.cpp
        icu/common/ucnv_bld.cpp
        icu/common/ucnv_ext.cpp
        icu/common/ucnv_io.cpp
        icu/common/ucnvbocu.cpp
        icu/common/ucnvsel.cpp
        icu/common/ucol_swp.cpp
        icu/common/udata.cpp
        icu/common/uhash_us.cpp
        icu/common/uidna.cpp
        icu/common/uinit.cpp
        icu/common/uiter.cpp
        icu/common/uloc.cpp
        icu/common/umutex.cpp
        icu/common/unames.cpp
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
        icu/common/uresbund.cpp
        icu/common/uscript_props.cpp
        icu/common/uset.cpp
        icu/common/uset_props.cpp
        icu/common/usetiter.cpp
        icu/common/ushape.cpp
        icu/common/usprep.cpp
        icu/common/ustack.cpp
        icu/common/ustr_titlecase_brkiter.cpp
        icu/common/ustr_wcs.cpp
        icu/common/ustrcase.cpp
        icu/common/ustrcase_locale.cpp
        icu/common/ustrenum.cpp
        icu/common/ustring.cpp
        icu/common/ustrtrns.cpp
        icu/common/utext.cpp
        icu/common/util.cpp
        icu/common/util_props.cpp
        icu/common/utrie.cpp
        icu/common/utrie2.cpp
        icu/common/utrie2_builder.cpp
        icu/common/uts46.cpp
        icu/common/uvector.cpp
        icu/common/uvectr32.cpp
        icu/common/uvectr64.cpp
        icu/stubdata/stubdata.c
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
    if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
        target_compile_options(icucommon
          PRIVATE
          -wd4267
          -wd4244)
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
set(HARFBUZZ_SRC_FILES
    ${HARFBUZZ_UNICODE_SRC}
    harfbuzz/src/hb-ucdn/ucdn.c
    harfbuzz/src/hb-ucdn.cc
    harfbuzz/src/hb-blob.cc
    harfbuzz/src/hb-buffer.cc
    harfbuzz/src/hb-common.cc
    harfbuzz/src/hb-face.cc
    harfbuzz/src/hb-font.cc
    harfbuzz/src/hb-set.cc
    harfbuzz/src/hb-shape.cc
    harfbuzz/src/hb-shape-plan.cc
    harfbuzz/src/hb-shaper.cc
    harfbuzz/src/hb-unicode.cc
    harfbuzz/src/hb-warning.cc
    harfbuzz/src/hb-ot-tag.cc
    harfbuzz/src/hb-ot-font.cc
    harfbuzz/src/hb-ot-layout.cc
    harfbuzz/src/hb-ot-map.cc
    harfbuzz/src/hb-ot-shape.cc
    harfbuzz/src/hb-ot-shape-complex-arabic.cc
    harfbuzz/src/hb-ot-shape-complex-default.cc
    harfbuzz/src/hb-ot-shape-complex-hangul.cc
    harfbuzz/src/hb-ot-shape-complex-hebrew.cc
    harfbuzz/src/hb-ot-shape-complex-indic.cc
    harfbuzz/src/hb-ot-shape-complex-indic-table.cc
    harfbuzz/src/hb-ot-shape-complex-myanmar.cc
    harfbuzz/src/hb-ot-shape-complex-thai.cc
    harfbuzz/src/hb-ot-shape-complex-tibetan.cc
    harfbuzz/src/hb-ot-shape-complex-use.cc
    harfbuzz/src/hb-ot-shape-complex-use-table.cc
    harfbuzz/src/hb-ot-shape-normalize.cc
    harfbuzz/src/hb-ot-shape-fallback.cc
    harfbuzz/src/hb-ot-var.cc)

list(APPEND HARFBUZZ_SRC_FILES "${CMAKE_CURRENT_SOURCE_DIR}/config.h")
add_library(harfbuzz ${HARFBUZZ_SRC_FILES})

if(CMAKE_CXX_COMPILER_ID STREQUAL "MSVC")
  set_source_files_properties(${HARFBUZZ_SRC_FILES}
     PROPERTIES
     COMPILE_FLAGS  "-utf-8 -wd4996 -wd4267 -wd4244")
endif()

set(HARFBUZZ_INCLUDE_DIRS
    ${CMAKE_CURRENT_SOURCE_DIR}/harfbuzz/src
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/include/
    ${HARFBUZZ_UNICODE_INCLUDE}
    ${CMAKE_CURRENT_SOURCE_DIR}/
    CACHE INTERNAL "Harfbuzz include dirs" FORCE)

target_include_directories(harfbuzz
    PUBLIC
    ${CMAKE_CURRENT_SOURCE_DIR}/harfbuzz-generated
    ${CMAKE_CURRENT_SOURCE_DIR}/harfbuzz/src
    ${CMAKE_CURRENT_SOURCE_DIR}/freetype/include
    ${HARFBUZZ_UNICODE_INCLUDE}
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

if (CMAKE_CXX_COMPILER_ID MATCHES "Clang")
  target_compile_options(harfbuzz
    PRIVATE
    -Wno-vla-extension
    -Wno-extended-offsetof)
endif()

target_compile_definitions(harfbuzz PUBLIC
    PRIVATE
    -DHAVE_OT
    # Provide own config file
    -DHAVE_CONFIG_H
    ${HARFBUZZ_UNICODE_DEFINE}
    -DHB_NO_MT)

