# Template Project for C++

## Build System: CMake

## Testing: Catch2

## Directory Layout
    .
    ├── src                     # Source files
    ├── tests                   # Automated tests
    ├── cmake                   # cmake scripts
    ├── CMakeLists.txt			# build config
    ├── LICENSE
    └── README.md


## Requirements:
- cmake
- gcc / clang / msvc (A C++ compiler)

## Usage
```bash
mkdir build
cd build
cmake ..

```

## Remarks
This project uses the v3 branch of Catch2 [ref](https://github.com/catchorg/Catch2/blob/devel/docs/migrate-v2-to-v3.md#top)
Catch2 is built as a static library and then linked to our tests executable.
