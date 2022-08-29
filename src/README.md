Normally there is be a README here. This is just for descriptive purposes.

This is the root directory of your Python package. It contains the source code
of your project in an appropriately-name directory. It doesn't have to be the
same name as the repo, but it should be relevant, concise, and _must not
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
