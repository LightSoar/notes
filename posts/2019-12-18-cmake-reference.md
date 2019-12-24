# Basics
## Build
Out-of-source-build inside `build` folder:
```bash
~/path/to/repo $ cmake -S . -B build # -S for source dir, -B for build dir
~/path/to/repo $ cmake --build build
~/path/to/repo $ cmake --build build --target install
```
## Debug
`cmake --build build`:
* `-v`
* `--trace`
* `--trace-source="filename"`

Additional flags:
* `-v`: verbose
* `-j N`: parallel builds on N cores
* `--trace [--trace-source="filename"]`: debug [individual] CMake files

## Do not use
* Global functions: `link_directories`, `include_libraries`
* Commands requiring to re-run cmake: GLOB
* Do not link to explicit paths (link to targets)

## Do use
* Link to targets (do not link to explicit paths)

## Configuration over convention
* Do not skip PRIVATE/PUBLIC when linking.

# Patterns
## Single-folder executable
```cmake
cmake_minimum_required(VERSION 3.1)

project(MyProject VERSION 1.0
                  DESCRIPTION "Very nice project"
                  LANGUAGES C CXX)
                  
add_executable(one two.cpp three.h)
```

## Single-folder library
```cmake
add_library(one STATIC two.cpp three.h)
add_library(another STATIC another.cpp another.h)
target_link_libraries(another PUBLIC one)
```

* PUBLIC doesn't mean much for an executable; for a library it lets CMake know that any targets that link to this target must also need that include directory.


# References
* [An intoduction to modern cmake](https://cliutils.gitlab.io/modern-cmake/)
