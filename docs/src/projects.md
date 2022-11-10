# Projects

The emscripten-forge ecosystem is split up into multiple components living in
individual repositories.

## recipes

Emscripten-forge is using the conda/mamba ecosystem to build the packages form wasm.
For each package we have a so called "recipe" which describes how the packages is build.
The [recipes](https://github.com/emscripten-forge/recipes/) repository is a collection of 
these recipes. New packages need to be added to to the recipes repo to be available on emscripten-forge.




## empack

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



## pyjs


## pyjs-code-runner


Debugging, experimenting and testing python code from with multiple conda environment 
in browser environment is a complex process with a lot of (complicated) steps. 
To run code from a script one has to:

* Create conda/mamba environments for emscripten-32
* Pack the conda/mamba environment with emscriptens [empack](https://github.com/emscripten-forge/empack)
* Pack the code / scripts you want to run [empack](https://github.com/emscripten-forge/empack)
* Pack all the assets / data / directories with [empack](https://github.com/emscripten-forge/empack)
* Create a javascript file which uses the [pyjs](https://github.com/emscripten-forge/pyjs) to run the the python script from JavaScript.
* Create a html file to run JavaScript code with `pyjs`
* Put everything in a directory and start a server with `python -m http.server` or simmilar
* Open a browser and observe the results of the script from the created `html` file or the console of the browser.

Of course, when chaging the script we do not need to run all of these steps, but overall prodecure is way to complicated to be enjoyable.


[pyjs-code-runner](https://github.com/emscripten-forge/pyjs-code-runner) is a driver to run python code in a wasm environment, almost like running vanilla python code. 
It allows to run python wasm code in the browser from a normal terminal like: 

```bash
# run with node
pyjs_code_runner run script                                                                     \
    node                                                                                        \
    --conda-env     ~/micromamba/envs/my_env         `# the emscripten-forge env`               \
                                                     `# in which to run the code`               \
                                                                                                \
    --mount         ~/foo/bar:/home/web_user/fubar   `# Mount path to virtual filesytem`        \
                                                     `# <HOST_MACHINE_PATH>:<TARGET_PATH>`      \
                                                                                                \
    --script        main.py                          `# Path of the script to run`              \
                                                     `# (in virtual filesystem)`                \
                                                                                                \
    --work-dir      /home/web_user/fubar             `# Work directory `                        \
                                                     `#in the virtual fileystem`                \
                                                                                                \
    --async-main                                     `# should a top-level async`               \
                                                     `# function named main be called`          \
                                                                                                \
    --headless                                       `# use a headless browser to run the code` \

```




## xeus-python-lite


[xeus-python](https://github.com/jupyterlite/xeus-python-kernel)



## picomamba

Picomamba is still a work in progress!

[picomamba](https://github.com/mamba-org/picomamba)