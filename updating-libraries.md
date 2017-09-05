# Updating harfbuzz

## Update subtree
1. Add harfbuzz as remote to your local repo
```bash
git remote add <remote name> https://github.com/behdad/harfbuzz
```
2. Update harfbuzz to appropriate release tag
```bash
git subtree pull --prefix=harfbuzz --squash <remote name> <release tag>
```
## Update harfbuzz-generated
1. Build harfbuzz to get generate shape file information
```bash
cd harfbuzz
NOCONFIGURE=1 ./autogen.sh
./configure --with-freetype --with-icu
make
```
2. compare and copy generated files in `harfbuzz-generated`

# Updating ICU

We update `ICU` project from the main ICU source page directly: http://source.icu-project.org/repos/icu/

# Updating Freetype

We update `Freetype` project from the main Freetype source page directly: https://www.freetype.org/download.html
