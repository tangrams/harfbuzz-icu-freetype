# harfbuzz-icu-freetype
Harfbuzz with a CMake build configuration using Freetype2, UCDN and ICU

To use in your project, simply add the following to your cmake configuration:

```CMake
# Harfbuzz ICU Freetype
add_subdirectory(${PROJECT_SOURCE_DIR}/harfbuzz-icu-freetype)

# Link libraries
add_executable(my_project.out ${SOURCES})
target_link_libraries(my_project.out freetype harfbuzz icucommon)
```

Where `harfbuzz-icu-freetype` is the root folder of this project.

An example project using this configuration can be found [here](https://github.com/tangrams/harfbuzz-example)

**options**

The following options can be given:

- `HARFBUZZ_BUILD_ICU`: builds Harfbuzz with ICU
- `HARFBUZZ_BUILD_UCDN`: builds Harfbuzz with UCDN
