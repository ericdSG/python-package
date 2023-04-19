# :package: Python Package

Basic directory structure required to package your Python project.

# :gear: Installation

Clone the repo, navigate to the top-level directory, and:

1. Initialize submodules

    ```bash
    git submodule update --init --recursive
    ```

1. Create a new conda environment `package-demo` with dependencies from
`environment.yml`

    ```bash
    mamba env create && mamba activate package-demo
    ```

1. Install this repo as a package in editable mode into your conda environment
using pip

    ```bash
    pip install -e .
    ```

    > **Note**
    > 
    > Editable mode (`-e`) means that any local changes take effect immediately.
    > The trailing `.` will install the current directory, so make sure you are
    > in the repo top-level directory.

# :rocket: Testing your installation works

Ensure the conda environment `package-demo` is activated, open the interactive
Python interpreter, import the test method, and try it out!

```bash
(package-demo) python-package $ python
>>> from python_package.test_module import test_add_one
>>> test_add_one(2)
1
```

Oh, no! There is a bug. :bug: Exit the interactive interpreter (<kbd>CTRL</kbd> + <kbd>D</kbd>),
fix the bug, and repeat the steps above to see your change!

# :children_crossing: How packaging works

> **Note**
> 
> Based on `pip>=21.3` and `setuptools>=64.0`

Installing a package with setuptools requires:
- `pyproject.toml`: recently-adopted configration file standard ([PEP 621](https://peps.python.org/pep-0621/),
[doc](https://packaging.python.org/en/latest/specifications/declaring-project-metadata/))
  - Must contain, at minimum: name, version
- `src/your_project_name/`: directory containing all of your project's source code

That's it! You want the top-level of the repo to be clean and free of clutter,
with only the necessary configuration files (for example, GitHub workflows,
pre-commit hooks, etc).

When you run `pip install -e .`, the setuptools backend will collect the
project metadata from `pyproject.toml` and add the contents of `src/` to your
environment. It will create a new directory `src/python_project.egg-info/`
that contains metadata for your project and should be `.gitignore`'d.

Note that there are other ways to package Python projects, such as Flit, Hatch,
or Poetry, but the simplest and traditional way is using pip + setuptools. At
the time of writing, conda does not yet support installing in editable mode but
you can still install a repo into a conda environment using pip as described
above.

> **Warning**
> 
> Using `pyproject.toml` instead of `setup.py` and `setup.cfg` is a recent
> change as of summer 2022, so any information prior to this related to
> packaging is likely outdated.

# :bulb: Other recommendations

This repo describes the necessary files and directory structure to create a valid
Python package, but you will certainly want to create other directories to
organize your code. There are a few considerations to keep in mind:

- Python source code (modules that you want to import into any executable
script/applications) should go in `src/`. Anything outside this directory will
not be packaged by setuptools.
- It is bad practice to commit data directly to git. It is better to use
another service like DVC or git-lfs.
  - Consider having a single top-level `data/` directory that contains only and
  all the input/output data for your project (i.e. no processing scripts). Don't
  forget to add `data/` to `.gitignore`!
- The principle holds for other kinds of content, such as experiment runs.
- To help users get off the ground quickly, add a (concise!) section in your
README with copy/paste steps for:
  - Installation: submodule initialization, conda environment setup, pip
package install
  - Basic usage: a minimal working example of how to get your code running
