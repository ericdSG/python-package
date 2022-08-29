# Python Project

Basic directory structure required to package your Python project.

Motivation:
- Standarize Python code
- Flexible module imports

## Installation

:rocket: **TIP**: Use [`mamba`](https://github.com/mamba-org/mamba) instead
of `conda` to significantly increase installation speed. If you don't already
have it installed: `conda install mamba -n base -c conda-forge`

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

Install any other dependencies you might need. In this case, let's assume
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

## Packaging

Tested with `pip=22.2.2` and `setuptools=59.8.0`. Note that using
`pyproject.toml` instead of `setup.py` and `setup.cfg` is a recent change in
summer 2022.

There are many ways to package Python projects, but the simplest and oldest way
is using `pip`/`setuptools`. At the time of writing, conda does not yet support
installing in editable mode (but you can still install into a conda
environment).

`setuptools` requires:
- `pyproject.toml`: Recently adopted configration file standard (see 
[PEP 621](https://peps.python.org/pep-0621/).)
- `src/your_project_name/` directory containing all of your project source
code.

That's it! You want the top-level of the repo to be clean, with only the
necessary configuration files (for example, GitHub workflows, pre-commit hooks,
etc).

When you run `pip install -e .`, the `setuptools` backend will collect the
project metadata from `pyproject.toml` and add the contents of `src/` to your
environemnt. It will create a new directory `src/python_project.egg-info/`
that contains the binaries for your project and should be `.gitignore`'d.

## Testing your installation works

Ensure the conda environment (`package-demo`) is activate, and open the
interactivate Python interpreter:

```
(package-demo) python-package $ python
Python 3.9.10 | packaged by conda-forge | (main, Feb  1 2022, 21:28:27)
[Clang 11.1.0 ] on darwin
Type "help", "copyright", "credits" or "license" for more information.
>>> 
```

Import the test method we defined and try it out!

```
>>> from python_package.test_module import test_add_one
>>> test_add_one(2)
3
```

## Other data

This repo only contains the necessary directories to create a valid Python
package from source code. You will certainly want to create other directories.
In general, it is bad practice to commit data directly to git. It is better to
use another service like DVC or git-lfs.

One solution to make it easier to keep track of data is to have a single
top-level `data/` directory that contains _only_ data (i.e. no data processing
scripts). Don't forget to add `data/` to `.gitignore`!

The same principle holds for other kinds of content, such as experiment runs.
