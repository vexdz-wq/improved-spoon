# Copilot Instructions for GitVexdz-Cmake4ish

## Project Overview
This is a CMake-based C/C++ project with a focus on modular, maintainable build systems. The project uses modern CMake (3.15+) conventions.

## Directory Structure
```
.
├── src/          # Source files (C/C++)
├── include/      # Public headers
├── tests/        # Unit tests
├── cmake/        # CMake modules and utilities
├── build/        # Build output (generated, not committed)
└── CMakeLists.txt
```

## Build & Development Workflow

### Local Development
```bash
# Initialize build directory
mkdir build && cd build

# Configure with CMake
cmake ..

# Build the project
cmake --build .

# Run tests (once implemented)
ctest
```

### Build Configuration
- **Default**: Debug build with symbols for development
- **Release**: Use `cmake -DCMAKE_BUILD_TYPE=Release ..` for optimized builds
- Clean builds: Remove `build/` directory entirely, don't use `cmake --build . --target clean`

## CMake Conventions

### Project Setup
- All `CMakeLists.txt` files use `cmake_minimum_required(VERSION 3.15)`
- Project name defined once in root `CMakeLists.txt`
- Use `add_library()` for shared code, `add_executable()` for applications
- Always specify `PUBLIC`, `PRIVATE`, `INTERFACE` on include directories and link dependencies

### Example Pattern
```cmake
add_library(core STATIC src/core.cpp)
target_include_directories(core PUBLIC ${CMAKE_CURRENT_SOURCE_DIR}/include)
target_link_libraries(core PRIVATE fmt::fmt)

add_executable(app src/main.cpp)
target_link_libraries(app PRIVATE core)
```

## File Organization Patterns

### Headers
- Put public headers in `include/` mirroring the library structure
- Use header guards: `#pragma once` (preferred) or `#ifndef` guards
- Forward declare when possible to reduce compile times

### Source Files
- Keep `.cpp` files in `src/` organized by subsystem
- One main component per file or group related utilities together
- Include path: `#include "mylib/component.h"` (relative to `include/`)

## Git Workflow

### Commit Messages
- Format: `<type>: <short description>` (50 chars max)
- Types: `feat:`, `fix:`, `refactor:`, `test:`, `docs:`, `build:`
- Example: `feat: add matrix multiplication support`

### Branch Strategy
- `main` branch is production-ready
- Feature branches: `feature/description` or `fix/issue-number`
- Merge via pull request with CI checks

## Testing Pattern
- Tests live in `tests/` directory
- Use CMake's `add_test()` or ctest for test discovery
- Test naming: `test_<component>.cpp`
- Run tests with `cmake --build . --target test` or `ctest`

## Code Style (Implement Once Files Exist)
- **Naming**: snake_case for variables/functions, PascalCase for classes
- **Formatting**: 2-space indentation, 100-char line limit
- Use clang-format with `.clang-format` config (create if missing)
- Run: `clang-format -i <file>`

## Dependencies
- External dependencies via `find_package()` in CMakeLists.txt
- Pin versions in `.github/dependabot.yml` or document in README
- Document setup instructions for each dependency

## Common Tasks

### Adding a New Library
1. Create `src/newlib/` and `include/newlib/`
2. Add `add_subdirectory(src/newlib)` to parent `CMakeLists.txt`
3. Create `src/newlib/CMakeLists.txt` with targets
4. Link in dependent targets via `target_link_libraries()`

### Adding Tests
1. Create `tests/test_<component>.cpp`
2. Add to root `CMakeLists.txt`:
   ```cmake
   enable_testing()
   add_subdirectory(tests)
   ```
3. Add test target: `add_test(NAME test_name COMMAND test_exe)`

### Debugging
- Build with debug symbols: `cmake -DCMAKE_BUILD_TYPE=Debug ..`
- Use lldb/gdb with executables in `build/` directory
- Add `message()` statements for CMake debugging

## Ignored Patterns
Files in `.gitignore` should NOT be committed:
- `build/` - generated build artifacts
- `.vscode/` and `.idea/` - IDE-specific configs
- `.swp`, `.swo` - editor swap files

## Integration Pattern
- No hard-coded paths - use CMake variables like `${CMAKE_SOURCE_DIR}`
- Avoid absolute paths; prefer relative paths and CMake cache variables
- Document external service dependencies in README if any

## Initial Setup Checklist
- [ ] Configure `.clang-format` file for code style
- [ ] Set up CI/CD in `.github/workflows/`
- [ ] Create `cmake/` directory with any custom modules
- [ ] Add example source files in `src/` and `include/`
- [ ] Implement first tests in `tests/`

---

*Last updated: 2026-02-07*  
*For CMake documentation: https://cmake.org/documentation/*
