empack
================================



Often programs need to have access to a filesytem to work properly. 
Once of these programs is Python:
Python contains built-in modules written in C, but also modules written in Python. The latter one
are stored as plain `*.py` files in the filesystem. 
When we run a simple Python script like:

```python
print('hello world')
```

the Python interpreter needs access a filesystem to use files like  `$PREFIX/lib/python3.9/encodings/uft_8.py`.
But when running wasm code in the browser, this code is sandboxed and has **no
access to the computers filesystem**. 
To overcome this limitation, emscripten provides serveral "virtual" filestems and comes with a [File System API](https://emscripten.org/docs/api_reference/Filesystem-API.html).
To run code which needs access to certain files, we need to "pack" st. they can be used with emscriptens virtual filesystem.
This [packaging](https://emscripten.org/docs/porting/files/packaging_files.html) is done by emscriptens [file-packager](https://github.com/emscripten-core/emscripten/blob/main/tools/file_packager.py) script.

In a nutshell, the file-packager does the following:
It takes directories/files as input and create two files: `<outname>.data`  and `<outname>.js`.
The `<outname>.data` contains the (compressed) content of the input files.
The `<outname>.js` file contains code to read the data of `<outname>.data` and populate the
virtual filesystem of emscripten forge with this content.





Ontop of these file-packaging capabilities provides by emscripten, we implemented [empack](https://github.com/emscripten-forge/empack)
which is a high level wrapper around the [file-packager](https://github.com/emscripten-core/emscripten/blob/main/tools/file_packager.py) script.
`empack` has the following features:

* python API
* CLI API    
* Pack files and directories 
* Pack a conda/mamba environment
* Filter API st. only desired files end packaged to save space
* Pack a distribution from repodata




.. toctree::
   :maxdepth: 2
   :caption: empack:

