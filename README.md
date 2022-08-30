# Python Package :package:

Basic directory structure required to package your Python project.

## Motivation

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

## Testing your installation works

Ensure the conda environment (`package-demo`) is activated and open the
interactive Python interpreter:

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

Even though we opened the interactive interpreter from within the repo,
we can import this method in the same way regardless of where the code
is executed from!

## How packaging works

:warning: Tested with `pip=22.2.2` and `setuptools=59.8.0`. Note that using
`pyproject.toml` instead of `setup.py` and `setup.cfg` is a recent change as of
summer 2022, so there is a lot of outdated information out there.

There are many ways to package Python projects, such as using Flit or Hatch,
but the simplest and oldest way is using pip + setuptools. At the time of
writing, conda does not yet support installing in editable mode but you can
still install a repo into a conda environment using pip.

Installing a package with setuptools requires:
- `pyproject.toml`: Recently adopted configration file standard (see 
[PEP 621](https://peps.python.org/pep-0621/))
- `src/your_project_name/`: directory containing all of your project's source
code

That's it! You want the top-level of the repo to be clean and free of clutter,
with only the necessary configuration files (for example, GitHub workflows,
pre-commit hooks, etc).

When you run `pip install -e .`, the setuptools backend will collect the
project metadata from `pyproject.toml` and add the contents of `src/` to your
environemnt. It will create a new directory `src/python_project.egg-info/`
that contains the binaries for your project and should be `.gitignore`'d.

## Other data

This repo only contains the necessary directories to create a valid Python
package from source code. You will certainly want to create other directories.
There are a few considerations to keep in mind:

- Source code should always go in `src/`
- In general, it is bad practice to commit data directly to git. It is better
to use another service like DVC or git-lfs.

## Recommendations

- Have a _**concise**_ section in your README with copy/paste steps for:
  1. Installation (submodule initialization, conda env, pip package install)
  2. Basic usage (a minimal working example of how to get your code running)
- Have a single top-level `data/` directory that contains only and all the data
for your project (i.e. no processing scripts). Don't forget to add `data/` to
`.gitignore`!
- The same principle holds for other kinds of content, such as experiment runs.
