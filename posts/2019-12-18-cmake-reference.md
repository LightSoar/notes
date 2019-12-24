# Basics
## Build
Out-of-source-build inside `build` folder:
```bash
~/path/to/repo $ cmake -S . -B build -L # -S for source dir, -B for build dir, -L to list variables in use
~/path/to/repo $ cmake --build build
~/path/to/repo $ cmake --build build --target install
```
## Debug
* `cmake --build build`:
   * `-v`
   * `--trace`
   * `--trace-source="filename" --trace-expand` (--trace-expand will exapnd variables)
* `message(STATUS "MY_VARIABLE=${MY_VARIABLE}")`

Additional flags:
* `-v`: verbose
* `-j N`: parallel builds on N cores
* `--trace [--trace-source="filename"]`: debug [individual] CMake files

## Do not use
* Global functions: `link_directories`, `include_libraries`
* Commands requiring to re-run cmake: GLOB
* Do not link to explicit paths (instead, link to targets; use find_library)


# References
* [An intoduction to modern cmake](https://cliutils.gitlab.io/modern-cmake/)
