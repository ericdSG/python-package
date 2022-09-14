:warning: Usually there is no README here. This is just for descriptive purposes.

This is the root directory of your Python package. It contains the source code
of your project in an appropriately-name directory. It doesn't have to be the
same name as the repo, but it should be relevant, succinct, and _must not
contain hyphens_! The name of this directory is what will be used in module
imports. In this case:

```
from python_package.test_module import test_add_one
```

Any other submodules/libraries/dependencies should be placed alongside your
package so that you can import the same way:

```
from MLtools.AudioLoader.src.core import AudioLoader
```

Source directories containing subdirectories should have an `__init__.py` file,
which is used by the Python interpreter to index other modules (files) in the
package.

You may have noticed that `MLtools` has `__init__.py` in the top-level. This is
because it was intended to be an importable library in this way. However
`MLtools` should be updated to be a "proper" Python package, which means
`__init__.py` will be removed and it will need to be installed separately with
`pip install -e ./src/MLtools/`.
