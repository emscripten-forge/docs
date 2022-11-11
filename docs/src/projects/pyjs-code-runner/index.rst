pyjs-code-runner
================================

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




.. toctree::
   :maxdepth: 2
   :caption: pyjs-code-runner:

   local_build.md