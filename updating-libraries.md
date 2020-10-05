# Updating harfbuzz

- Get the latest release source code from: https://github.com/behdad/harfbuzz

- Remove the `tests` folder (it contains a large amount of font data that isn't useful for this repo).

- Update the Harfbuzz section of [CMakeLists.txt](CMakeLists.txt) for any new or moved source files. Also check the Harfbuzz [CMakeLists.txt](harfbuzz/CMakeLists.txt) for new compile definitions and flags.

# Updating ICU

- Get the latest release source code from: https://github.com/unicode-org/icu

- Take only the `source/common` and `source/stubdata` folders and copy them into `icu/`.

- Update the ICU section of [CMakeLists.txt](CMakeLists.txt) for any new or moved source files.

# Updating Freetype

- Get the latest release source code from: https://www.freetype.org/download.html

- That's it!
