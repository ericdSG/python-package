# Python Package :package:

Basic directory structure required to package your Python project.

## Motivation

- Standarize Python code
- Flexible module imports

## Installation

:rocket: **TIP**: Use [mamba](https://github.com/mamba-org/mamba) instead
of conda to significantly increase installation speed. If you don't already
have it installed: `conda install mamba -n base -c conda-forge`. Alternatively,
replace all `mamba` commands with `conda`.

Clone the repo:

```
git clone git@github.com:ericdSG/python-package.git
cd python-package/
```

Initialize submodules:

```
git submodule update --init --recursive
```

Create a conda environment from the env file. The env name and
Python version are already defined in the file.

```
mamba env create --file environment.yaml
mamba activate package-demo
```

Optional: install any other dependencies you might need. Let's assume
you will be using the AudioLoader:

```
mamba env update --file src/MLtools/AudioLoader/environment.yml
```

Install this repo as a package in editable mode (`-e`) into the conda 
environment using pip. Editable mode means that any local changes take effect
immediately. The trailing `.` will install the current directory, so make sure
you are in the repo top-level directory.

```
pip install -e .
```

## Testing your installation works

Ensure the conda environment (`package-demo`) is activated and open the
interactive Python interpreter:

```
(package-demo) python-package $ python
Python 3.7.12 | packaged by conda-forge | (default, Oct 26 2021, 05:57:50)
[Clang 11.1.0 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

Import the test method we defined and try it out!

```
>>> from python_package.test_module import test_add_one
>>> test_add_one(2)
1
```

Oh, no! There is a bug. :bug: Exit the interactive interpreter, fix the bug,
re-open the interpreter, and import the function again to see your change!

## How packaging works

:warning: Note that using `pyproject.toml` instead of `setup.py` and `setup.cfg`
is a recent change as of summer 2022, so there is a lot of outdated information
out there.

There are many ways to package Python projects, such as Flit or Hatch, but the
simplest and oldest way is using pip + setuptools. At the time of writing,
conda does not yet support installing in editable mode but you can still
install a repo into a conda environment using pip.

Installing a package with setuptools requires:
- `pyproject.toml`: Recently adopted configration file standard (see 
[PEP 621](https://peps.python.org/pep-0621/))
  - Must contain, at minimum: `build-backend`, `requires`, `name`, `version`
- `src/your_project_name/`: directory containing all of your project's source
code

That's it! You want the top-level of the repo to be clean and free of clutter,
with only the necessary configuration files (for example, GitHub workflows,
pre-commit hooks, etc).

When you run `pip install -e .`, the setuptools backend will collect the
project metadata from `pyproject.toml` and add the contents of `src/` to your
environment. It will create a new directory `src/python_project.egg-info/`
that contains metadata for your project and should be `.gitignore`'d.

:rotating-light: Tested with `pip=22.2.2` and `setuptools=59.8.0`. If you have issues,
ensure that `pip>=21.3`. Failing that: `setuptools>=64.0`.

## Other recommendations

This repo describes the necessary files and directory structure to create a valid
Python package, but you will certainly want to create other directories to
organize your code. There are a few considerations to keep in mind:

- Python source code should always go in `src/`. Anything outside this directory
will not be packaged by setuptools.
- In general, it is bad practice to commit data directly to git. It is better
to use another service like DVC or git-lfs.
  - Consider having a single top-level `data/` directory that contains only and
  all the input/output data for your project (i.e. no processing scripts). Don't
  forget to add `data/` to `.gitignore`!
- The principle holds for other kinds of content, such as experiment runs.
- To help users get off the ground quickly, add a (concise!) section in your
README with copy/paste steps for:
  - Installation - submodule initialization, conda environment setup, pip
package install
  - Basic usage - a minimal working example of how to get your code running
