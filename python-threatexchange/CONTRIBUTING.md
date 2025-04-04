# Contributing

Please see [CONTRIBUTING](../CONTRIBUTING.md) in the repo root for more general guidelines on how to contribute by
developing locally and submitting pull requests on Github.

## Development environments

### Self-managed Python

If you're a regular Python developer and already have a Python development environment that works for you,
you can use it. `python-threatexchange` uses a `pyproject.toml` file for its build configuration where all dependencies are defined.

### macOS, Homebrew, virtualenv

Virtualenv is a commonly used tool in the Python ecosystem which creates lightweight isolated environments
for a specific project. This avoids conflicting package dependencies when working on multiple Python projects.

* Install Python - `brew install python`
* Install virtualenv - `pip install virtualenv`
* Create a virtualenv in this directory: `virtualenv .venv`
* Activate the virtualenv: `source .venv/bin/activate`
* Install `python-threatexchange` into the virtualenv in editable mode with dev extras: `pip install --editable '.[dev]'`
* You should now be able to run the CLI by executing the `threatexchange` or `tx` command within the activated venv.

### VSCode with a Dev Container

In some circumstances there are barriers to using the above workflow, such as managed laptops
which have restrictions on what can be installed. If you are a Meta employee using a corporate
laptop you will most likely run into difficulties for this reason.

In this case, you have the option to use a development container, which has tight integration with Visual Studio Code.
The configuration for this is defined in the `.devcontainer` directory.

To use this workflow:
* Install VSCode. (If you're a Meta employee, be sure to install the stock VSCode as your preinstalled version won't be compatible).
* Install Docker Desktop
* Install the "Dev Containers" extension in VSCode
* Clone this Git repo locally
* Open the `python-threatexchange` project in the dev container:
  * Cmd-Shift-P, "Dev Containers: Open Folder in Container..."
  * Select this directory within the local Git repo (**not** the parent directory, the repo root)
  * Wait for the container to build
* Selecting Terminal > New Terminal from the VSCode menu will open a terminal in the dev container.

This is a Debian based Linux container with all Python dependencies installed. The `threatexchange` and `tx` commands
map directly to the live code in your editor, so there is no need to reinstall or resync between edits.

## Code Style
threatexchange uses [black](https://PyPI.org/project/black/) for consistent formatting across
the projects python source files. After installing black locally, you can automatically
format all the python-threatexchange files by running the following command from the repository root.

```shell
black ./python-threatexchange/
```

Additionally, your IDE may have support for automatically re-formatting your source files
using black through your IDE settings.

When in doubt, follow the style of the file you are editing, and if starting new files, be consistent within.

## python typing and `mypy`
We are aiming for full strict coverage via PEP484 typing annotations. However, this is a work in progress. We use [mypy](https://mypy.readthedocs.io/en/stable/index.html) to do typechecking.

### `# type: ignore`
We are not all typing experts yet, and we've confused ourselves with Generics, Aliases and object hierarchies, and occasionally `# type: ignored`. If you end up in a sticky situation and decide to throw in the towel on typing and use `# type: ignore`, use `mypy --show-error-codes` to only ignore the specific error code, and expect discussion on the PR on the level of effort needed to make the code compliant.

### New Files or Directories
When creating new files or directories, please first try and make them compatible with `--strict` to do this, add in the mypy.ini file
```
[path.to.module]
strict = True
```
Read more about mypy configuration [here](https://mypy.readthedocs.io/en/stable/config_file.html#config-file).

## Tests
python-threatexchange uses [pytest](https://docs.pytest.org/en/7.1.x/) for unittesting. From the root python-threatexchange directory use 
```shell
py.test
```
to run tests.

### Writing Tests
New functionality should have unittests. Core functionality like SignalType and SignalExchangeAPI have good examples of tests in their various directories.

We prefer tests to live in their own files in the /tests/ directory where the code under test is.

### Skipped Tests
Many tests rely on extensions that are not installed by default when you install `threatexchange`. It is okay to submit a PR without running skipped tests, they will be run by workflow. If you find out that tests are failing, you can follow the instructions in the extensions directories to install the needed dependencies, which will make the tests runnable locally.

### Installing Locally
A quick way to iterate on the script is to simply install it locally. The
fastest way to do this is

    cd ThreatExchange/python-threatexchange
    make package
    make local_install
    threatexchange --help

## Releasing A New PyPI version
Releases of the library are managed by a [GitHub action](../.github/workflows/python-threatexchange-release.yaml), which are triggered by changes to [version.txt](./version.txt).

Version releases should be in a PR on their own, and not included with functional changes.

To create a new release to [PyPI](https://PyPI.org/project/threatexchange/), update [version.txt](./version.txt)
to the new release name in a PR. Once the PR is approved and merged, the CI process
will publish the new version to [PyPI](https://PyPI.org/), shortly after a test publish to Test PyPI.

### PyPI test instance versions
We sometimes do release candidate previews to the PyPI test instance. If you have the credentials for the threatexchange account, you can build and release a test package by heading to the python-threatexchange root directory and do:

```shell
$ make clean
$ make package
$ make push
```

## Extensions
We will only rarely add new extensions, which require additional dependencies, and are not enabled by default. We encourage authors to write and own their own extensions! Feel free to create PR's to add your own extensions to the list of known ones in the README.

# Writing Pull Requests
Thank you for considering writing improvements to the library! We accept pull requests!
Please see the detailed instructions in the root-level [CONTRIBUTING](../CONTRIBUTING.md) on making PRs.

The following are additional considerations or pytx-specific notes:

## Before Submitting a PR
Make sure to run all the local tests and linters. They should complete quickly:

```shell
# From the python-threatexchange root directory
$ black .
$ python -m mypy threatexchange
$ py.test
```
