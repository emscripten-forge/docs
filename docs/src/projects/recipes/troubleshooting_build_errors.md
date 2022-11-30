# Troubleshooting Build Errors

### Contents

1. [`sed: no input files`](#sed-no-input-files)
1. [Dynamic linking / shared libraries](#target-does-not-support-dynamic-linking)

---

## `sed: no input files`

**Error message**:
```bash
...
Setting up cross-python
sed: no input files

```


**Solution**:

An error message like the one above for packages using `cross-python` usually means
`python` is missing from the `host`-dependencies.

## Target does not support dynamic linking

**Error message**:
```bash
CMake Warning (dev) at CMakeLists.txt:XXX (add_library):
  ADD_LIBRARY called with SHARED option but the target platform does not
  support dynamic linking.  Building a STATIC library instead.  This may lead
  to problems.
```

**Solution**:

This solution is obtained from [emscripten-core/emscripten#15276](https://github.com/emscripten-core/emscripten/issues/15276#issuecomment-1039349267).

Create an *overwriteProp.cmake* file containing the following:
```cmake
set_property(GLOBAL PROPERTY TARGET_SUPPORTS_SHARED_LIBS TRUE)
set(CMAKE_SHARED_LIBRARY_CREATE_C_FLAGS "-s SIDE_MODULE=1")
set(CMAKE_SHARED_LIBRARY_CREATE_CXX_FLAGS "-s SIDE_MODULE=1")
set(CMAKE_STRIP FALSE)
```

Add the following option to `cmake`:
```bash
-DCMAKE_PROJECT_INCLUDE=$RECIPE_DIR/overwriteProp.cmake
```

