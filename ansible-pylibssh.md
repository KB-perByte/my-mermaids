# pylibssh

## Build/Test/Lint Commands
- `tox -e lint` - Run all linting checks (flake8, isort, yamllint, pre-commit)
- `tox` - Run full test suite with coverage in default Python environment
- `tox -e just-pytest` - Run pytest without coverage or Cython compilation
- `pytest tests/unit/test_filename.py::test_function` - Run single test
- `tox -e build-dists` - Build source and wheel distributions
- `tox -e clean` - Clean build artifacts and cached files

## Architecture
- **Cython extension**: Core SSH functionality in `src/pylibsshext/` (*.pyx, *.pxd files)
- **Python bindings**: Ansible-specific SSH client for libssh
- **Custom build backend**: PEP517 backend in `packaging/pep517_backend/`
- **Tests**: Unit tests in `tests/unit/`, integration tests in `tests/integration/`
- **Main package**: `src/pylibsshext/` contains the compiled extension module

## Code Style
- **Line length**: 160 characters (PEP8 unfriendly but accessibility-friendly)
- **Imports**: Use isort with sections: FUTURE, STDLIB, CYTHON, TESTING, THIRDPARTY, FIRSTPARTY, LOCALFOLDER
- **Formatting**: Use trailing commas, 4-space indentation, double quotes preferred
- **Cython**: Files in `src/pylibsshext/*.pyx` and `*.pxd` for C extensions
- **Error handling**: Follow wemake-python-styleguide standards
- **Testing**: Use pytest with coverage, fixtures, and parametrized tests
- **Pre-commit**: Required hooks include flake8, isort, yamllint, and custom changelog validation
