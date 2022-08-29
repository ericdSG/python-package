# Python Project

Basic directory structure required to package your Python project.

Motivation:
- Standarize Python code
- Flexible module imports

## Installation

:rocket: _**TIP**: Use [`mamba`](https://github.com/mamba-org/mamba) instead
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

Create a mamba (conda) environment from the environment file. The name and
Python version are already set in the file.

```
mamba env create --file environment.yaml
mamba activate package-demo
```

Install any other dependencies you might need. In this case, let's assume
you will be using the AudioLoader:

```
mamba env update --file src/MLtools/AudioLoader/environment.yml
```

Install this project as a package in editable mode (`-e`) the environment using
pip. Editable mode means that any local changes take effect immediately. The
trailing `.` will install the current directory, so make sure you are in the
repo top-level directory.

```
pip install -e .
```

## Packaging

There are many ways to package Python projects, but the simplest and oldest way
is using `pip`/`setuptools`. At the time of writing, conda does not yet support
installing in editable mode (but you can still install into a conda
environment).

`setuptools` requires:
- `pyproject.toml`: Recently adopted configration file standard (see 
[PEP 621](https://peps.python.org/pep-0621/).)
- `setup.cfg`: Former configuration file standard that, at the time of writing,
is still required to for editable mode install when using older versions of
`pip`/`setuptools`. Just leave it empty.
- `src/your_project_name/` directory containing all of your project source
code.

That's it! You want the top-level of the repo to be clean, with only the
necessary configuration files (for example, GitHub workflows, pre-commit hooks,
etc).

When you run `pip install -e .`, the `setuptools` backend will collect the
project metadata from `pyproject.toml` and add the contents of `src/` to your
environemnt.

## Other data

This repo only contains the necessary directories to create a valid Python
package from source code. You will certainly want to create other directories.
In general, it is bad practice to commit data directly to git. It is better to
use another service like DVC or git-lfs.

One solution to make it easier to keep track of data is to have a single
top-level `data/` directory that contains _only_ data (i.e. no data processing
scripts). Don't forget to add `data/` to `.gitignore`!

The same principle holds for other kinds of content, such as experiment runs.
