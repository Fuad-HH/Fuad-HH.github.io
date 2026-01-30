---
title: Binding C/C++ to Python for a Minimalistic Project
date: 2019-07-12
math: false
---

There are many ways to bind C/C++ code to Python, ranging from simple tools like `ctypes` and `cffi` to more sophisticated libraries like `pybind11`, `Cython`, and `SWIG`. However, these tools often come with their own learning curves and dependencies. But you may want to keep things clean, easily maintainable and minimalistic, especially for small projects or when you want to avoid adding extra dependencies. But I didn’t find a simple, step-by-step guide that shows how to do this from scratch.

{{% callout note %}}
Before starting, please note that there are many great tools for binding C/C++ to Python and packaging them. If you are interested in the fundamentals, you can start here with the packaging guide from the official Python documentation: https://packaging.python.org/en/latest/. It will guide you through best practices for packaging Python projects, including those with C/C++ extensions.
{{% /callout %}}

{{% callout note %}}
This tutorial is based on my recent package [readOH2csg](https://github.com/Fuad-HH/readOH2csg/tree/parallel). Although you may not be interested in the specific application, it can help illustrate the steps needed to create a minimal C/C++ to Python binding using only `ctypes` and CMake. This binding approach is based on [OpenMC's](https://github.com/openmc-dev/openmc/).
{{% /callout %}}

In this tutorial, I will show how to build a tiny Python “API” backed by C functions, without using any external bindings tools like pybind11, Cython, or SWIG. Instead, we will use only the built-in `ctypes` library in Python and CMake for building the shared library. The goal is to demonstrate the essential steps needed to call C/C++ code from Python with minimal dependencies.

1. Set up a simple project directory
1. Write a small C library with a clean C API
1. Build a shared library (.so) using CMake
1. Copy the library to a place Python can easily load it
1. Call the C functions from Python using only ctypes
1. Package it with a minimal pyproject.toml

The focus is on clarity and portability.

## 1. Project Structure
We’ll keep the code minimal and the steps explicit. First, let’s look at the project structure:

```text
minimal_ctypes_api/
├─ CMakeLists.txt
├─ pyproject.toml
├─ src/
│  ├─ C-C++/
│  │  ├─ mylib.cpp
│  │  └─ mylib.h
│  └─ pythonAPI/
│     ├─ minimal_ctypes_api/
|     |  ├─ lib/
|     |  |  ├─ __init__.py
|     |  |  └─ (built shared library goes here)
│     │  ├─ __init__.py
│     │  └─ bindings.py
│     └─ tests/
│        └─ test_basic.py
```

- src/C-C++ – C and C++ sources and headers (your C API lives here). This directory structure is flexible; you can organize it as you like.
- src/pythonAPI/minimal_ctypes_api – Python package.
- src/pythonAPI/tests – basic tests and examples.
- CMakeLists.txt – build configuration for the shared library.
- pyproject.toml – Python build & package configuration.

You can adjust names, but keeping C and Python code clearly separated helps a lot.

## 2. Writing a Minimal C API
We’ll expose a couple of simple functions:

- `add(int a, int b) -> int`
- `mean(double* data, int length) -> double`

The `mylib.h` header defines the API:

```c
#ifndef MYLIB_H
#define MYLIB_H

#ifdef __cplusplus
extern "C" {
#endif

int add_ints(int a, int b);

/**
 * Compute the mean of an array of doubles.
 * Returns 0.0 if length <= 0.
 */
double mean_double_array(const double* data, int length);

#ifdef __cplusplus
}
#endif

#endif // MYLIB_H
```

Key points:
- `extern "C"` makes the functions C‑linkage safe when compiled as C++.
- The API is simple and uses only primitive types and pointers.

The `mylib.cpp` file implements the functions:

```c++
#include "mylib.h"
#include <stdexcept>

extern "C" int add_ints(int a, int b) {
    return a + b;
}

extern "C" double mean_double_array(const double* data, const int length) {
    if (!data || length <= 0) {
        throw std::invalid_argument("Invalid data pointer or length");
    }

    double sum = 0.0;
    for (int i = 0; i < length; ++i) {
        sum += data[i];
    }
    return sum / (double)length;
}
```
Notice that we added `extern "C"` to the function definitions as well, ensuring that we can write C++ code while maintaining C linkage. Any C++ headers/functions can be included here as needed.

## 3. Building the Shared Library with CMake
We want CMake to:

1. Build a shared library (mylib).
2. Place it into a directory where Python code can find it (e.g. next to bindings.py).

For example, we’ll target this path at build time:
```text
src/python/minimal_ctypes_api/lib/
```

This way, our Python module can simply look for a library file in its own package.

### 3.1 Top-level CMakeLists.txt
```cmake
cmake_minimum_required(VERSION 3.15)

project(minimal_ctypes_api CXX)

# Options
set(CMAKE_CXX_STANDARD 11)
set(CMAKE_CXX_STANDARD_REQUIRED ON)

# Where to put the built shared library by default
# We'll install/copy into src/python/minimal_ctypes_api/lib
set(PYTHON_PACKAGE_DIR "${CMAKE_SOURCE_DIR}/src/python/minimal_ctypes_api")
set(PYTHON_LIB_DIR "${PYTHON_PACKAGE_DIR}/lib")

# Add the C/C++ library
add_library(mylib SHARED
    src/c-c++/mylib.cpp
)
target_include_directories(mylib PUBLIC
    "${CMAKE_SOURCE_DIR}/src/c-c++"
)
target_link_libraries(mylib
    # Add any required libraries here
)

# Add an explicit copy step after build (useful across generators)
add_custom_command(
    TARGET mylib
    POST_BUILD
    COMMAND
      ${CMAKE_COMMAND} -E copy $<TARGET_FILE:mylib>
      ${PROJECT_SOURCE_DIR}/src/pythonAPI/minimal_ctypes_api/lib/$<TARGET_FILE_NAME:mylib>
    COMMENT
      "Copying $<TARGET_FILE:mylib> to ${PROJECT_SOURCE_DIR}/src/pythonAPI/minimal_ctypes_api/lib/$<TARGET_FILE_NAME:mylib>"
)

# Other CMake configurations can go here
```

## 4. Python Bindings with `ctypes`

Now, with the shared library built and placed in the right directory, we can write Python bindings using `ctypes`.
In `pythonAPI/minimal_ctypes_api/lib/__init__.py`, we can leave it empty or add package-level docstrings.
Then in `pythonAPI/minimal_ctypes_api/__init__.py` we will load the dynamic library`_dll`:

```python
import importlib.resources
from ctypes import CDLL
import sys

# Find shared library
assert (sys.platform == "linux") #only works with linux

_filename = importlib.resources.files(__name__)/f'lib/libomegah2csg.so'
_dll = CDLL(str(_filename))
```

Now we can define Python functions that wrap the C functions in `bindings.py`:

```python
import numpy as np
from numpy.ctypeslib import ndpointer

from ctypes import c_int, c_double, c_bool
from . import _dll

# Define argument and return types for the C functions
_dll.add_ints.argtypes = [c_int, c_int]
_dll.add_ints.restype = c_int

_dll.mean_double_array.argtypes = [ndpointer(c_double, flags="C_CONTIGUOUS"), c_int]
_dll.mean_double_array.restype = c_double

def capi_add(a: int, b: int) -> int:
    try:
        return _dll.add_ints(a, b)
    except Exception as e:
        raise RuntimeError(f"Error calling add_ints: {e}")

def capi_mean(data: np.ndarray) -> float:
    if not isinstance(data, np.ndarray):
        raise TypeError("data must be a numpy array")

    if data.dtype != np.float64:
        raise TypeError("data array must be of type float64")

    length = data.size
    try:
        return _dll.mean_double_array(data, length)
    except Exception as e:
        raise RuntimeError(f"Error calling mean_double_array: {e}")
```

Note that we use `numpy.ctypeslib.ndpointer` and it makes passing NumPy arrays to C functions straightforward. See more in the [NumPy documentation](https://numpy.org/doc/stable/reference/routines.ctypeslib.html). We are also using try/except blocks to catch any exceptions raised from the C++ side and re-raise them as Python exceptions.

## 5. Packaging with `pyproject.toml`
Our python package is complete now. To package it, we can use the following minimal `pyproject.toml` example:

```toml
[build-system]
requires = ["setuptools>=61"]
build-backend = "setuptools.build_meta"

[project]
name = "minimal-ctypes-api"
version = "0.1.0"
description = "Python interface to a minimal C/C++ library using ctypes"
readme = "README.md"
requires-python = ">=3.8"

[tool.setuptools]
package-dir = {"" = "pythonAPI"}
packages = ["minimal_ctypes_api"]

[project.optional-dependencies]
test = [
    "pytest",
]

[tool.setuptools.package-data]
minimal_ctypes_api = ["lib/libmylib.so"]
```
This configuration tells setuptools to
1. Include the shared library in the package data.
2. Use the `pythonAPI` directory as the source for the package.

Find more details about this file in packaging documentation: https://packaging.python.org/en/latest/guides/writing-pyproject-toml/

## 6. Install the Package Using `pip`
To install the package locally for development, we need to create a `pip` environment. From a directory where you want to set up the environment, run:

```bash
python -m venv test-env
source test-env/bin/activate
pip install --upgrade pip setuptools build pytest
```
Then, navigate to the root of the project (where `pyproject.toml` is located) and run:

```bash
pip install -e .[test]
```
and it will build and install the package in editable mode.

## 7. Testing the Package
Now we can write a simple test to verify that everything works as expected. In `pythonAPI/tests/test_basic.py`, we can add:

```python
import numpy as np
from minimal_ctypes_api.bindings import capi_add, capi_mean

def test_add():
    assert capi_add(2, 3) == 5
    assert capi_add(-1, 1) == 0

def test_mean():
    data = np.array([1.0, 2.0, 3.0, 4.0, 5.0], dtype=np.float64)
    assert abs(capi_mean(data) - 3.0) < 1e-6

    empty_data = np.array([], dtype=np.float64)
    try:
        capi_mean(empty_data)
    except RuntimeError as e:
        assert "Invalid data pointer or length" in str(e)
```
You can run the tests using `pytest`:

```bash
pytest pythonAPI/tests/
```

This should run the tests and confirm that the C functions are correctly called from Python.

Now you have a python package that wraps a minimal C/C++ library using only `ctypes`, with a clear structure and build process!

