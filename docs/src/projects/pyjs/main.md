[Pyjs](https://github.com/emscripten-forge/pyjs) is C++ library, compiled to wasm, which allows to run Python code in the browser or in node.
Pyjs is a loosly based on [pyoide](https://pyodide.org/en/stable/) and comes with is a Javascript ⟺ Python foreign function interface so that you call these languages from each other.
We would love to just use Pyodide’s FFI  in emscripten forge, but it is very non-trivial to extract Pyodide’s FFI and use it for other projects. Therefore we created a modern Python - JavaScript FFI from scratch. This was done with the following tricks:

* [Pybind11](https://github.com/pybind/pybind11) is used to call Python from C++ and vice versa,
* [Embind](https://emscripten.org/docs/porting/connecting_cpp_and_javascript/embind.html) is used to call JavaScript from C++ and vice versa.

When we use Pybind11 and Embind together we can call Python from JavaScript and vice versa, with C++ as a man in the middle. This not only allows us to write a simple FFI from scratch with relatively little code but also avoids calling any low-level CPython APIs and enables using high-level constructs — like `pybind11::object` and `emscripten::val` — instead.
The API is very similar to Pyodide’s so that it can be (allmost) be used as a drop-in replacement.


