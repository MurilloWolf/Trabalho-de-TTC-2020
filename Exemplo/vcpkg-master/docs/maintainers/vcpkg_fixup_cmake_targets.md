# vcpkg_fixup_cmake_targets

Merge release and debug CMake targets and configs to support multiconfig generators.

Additionally corrects common issues with targets, such as absolute paths and incorrectly placed binaries.

## Usage
```cmake
vcpkg_fixup_cmake_targets([CONFIG_PATH <share/${PORT}>] [TARGET_PATH <share/${PORT}>])
```

## Parameters

### CONFIG_PATH
Subpath currently containing `*.cmake` files subdirectory (like `lib/cmake/${PORT}`). Should be relative to `${CURRENT_PACKAGES_DIR}`.

Defaults to `share/${PORT}`.

### TARGET_PATH
Subpath to which the above `*.cmake` files should be moved. Should be relative to `${CURRENT_PACKAGES_DIR}`.
This needs to be specified if the port name differs from the `find_package()` name.

Defaults to `share/${PORT}`.

## Notes
Transform all `/debug/<CONFIG_PATH>/*targets-debug.cmake` files and move them to `/<TARGET_PATH>`.
Removes all `/debug/<CONFIG_PATH>/*targets.cmake` and `/debug/<CONFIG_PATH>/*config.cmake`.

Transform all references matching `/bin/*.exe` to `/tools/<port>/*.exe` on Windows.
Transform all references matching `/bin/*` to `/tools/<port>/*` on other platforms.

Fix `${_IMPORT_PREFIX}` in auto generated targets to be one folder deeper.
Replace `${CURRENT_INSTALLED_DIR}` with `${_IMPORT_PREFIX}` in configs and targets.

## Examples

* [concurrentqueue](https://github.com/Microsoft/vcpkg/blob/master/ports/concurrentqueue/portfile.cmake)
* [curl](https://github.com/Microsoft/vcpkg/blob/master/ports/curl/portfile.cmake)
* [nlohmann-json](https://github.com/Microsoft/vcpkg/blob/master/ports/nlohmann-json/portfile.cmake)

## Source
[scripts/cmake/vcpkg_fixup_cmake_targets.cmake](https://github.com/Microsoft/vcpkg/blob/master/scripts/cmake/vcpkg_fixup_cmake_targets.cmake)
