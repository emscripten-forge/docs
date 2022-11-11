# Build recipes locally

Emscripten-forge recipes can be build locally.
To setup a local dev. environment one can either follow the steps of the [CI pipeline](https://github.com/emscripten-forge/recipes/blob/main/.github/workflows/build_recipes.yaml) or follow the steps below:


## Setup Local Build



Fork / Clone the [emscripten-forge/recipes](https://github.com/emscripten-forge/recipes) repository
```bash
git clone git@github.com:emscripten-forge/recipes.git
```

Go to the repository directory
```
cd recipes
```


Create the dev environment named `emforge` for the conda/mamba env file
[ci_env.yaml](https://github.com/emscripten-forge/recipes/blob/main/ci_env.yml)
```
micromamba create -n emforge -f ci_env.yaml
```

Activate the environment
```
micromamba activate emforge
```


Install dependencies where we need non-default branches/forkes.

```
python -m pip install git+https://github.com/DerThorsten/boa.git@postcb --no-deps --ignore-installed
python -m pip install git+https://github.com/DerThorsten/bitfurnace.git@emscripten_new --no-deps --ignore-installed
```

Install Playwright
```
playwright install
```


Install the emsdk:
this will download the emscripten toolchain / compilers.
```
python -c "from empack.file_packager import download_and_setup_emsdk; download_and_setup_emsdk()"
```

create a `.emsdkdir` file in the home
```
echo $(python -c "from empack.file_packager import EMSDK_INSTALL_PATH; print(EMSDK_INSTALL_PATH / 'emsdk-3.1.2')") > $HOME/.emsdkdir
```

Add  `https://repo.mamba.pm/emscripten-forge` and `conda-forge` channels to the `.condarc`
```
conda config --add channels conda-forge
conda config --add channels https://repo.mamba.pm/emscripten-forge
```

You can also manually do this by chaging your `.condarc`  st. it looks like:

```yaml
channels:
  - https://repo.mamba.pm/emscripten-forge
  - conda-forge
  
add_pip_as_python_dependency: false

```


## Bootrap Build Recipes

We are almost done. We now should be able build packages. But we want to create a local conda channel st. we install
and use the packages we build localy. To do this, we first need build an arbitrary package. This
will popoulate the `conda-bld` directory of our `emforge` environment (ie create and fill the `$MAMBA_ROOT_PREFIX/envs/emforge/conda-bld` folder).

As an exemplary recipe we use [recipes/recipes_emscripten/bzip2](https://github.com/emscripten-forge/recipes/tree/main/recipes/recipes_emscripten/bzip2). 
We need to pass `--skip-tests` because the test mechanism will only work aftger we created the local conda channel.

```
python builder.py build explicit  recipes/recipes_emscripten/bzip2    --skip-tests
```

One the build is done we can create the local channel by adding   `$MAMBA_ROOT_PREFIX/envs/emforge/conda-bld` to condarc

```
conda config --add channels $MAMBA_ROOT_PREFIX/envs/emforge/conda-bld
```

You can also manually do this by chaging your `.condarc`  st. it looks like (change `/home/username/micromamba-root` to the value of `$MAMBA_ROOT_PREFIX )

```yaml
channels:
  - /home/username/micromamba-root/envs/ci-env/conda-bld/
  - https://repo.mamba.pm/emscripten-forge
  - conda-forge
  
add_pip_as_python_dependency: false

```

## Build Recipes


Build a package 

```
python builder.py build explicit   recipes/recipes_emscripten/bzip2    
```