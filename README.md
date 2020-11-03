# `arduino/cpp-test-action`

[![Validate](https://github.com/arduino/cpp-test-action/workflows/Validate%20action.yml/badge.svg)](https://github.com/arduino/cpp-test-action/actions?workflow=Validate+action.yml)
[![Spell Check](https://github.com/arduino/cpp-test-action/workflows/Spell%20Check/badge.svg)](https://github.com/arduino/cpp-test-action/actions?workflow=Spell+Check)

A [GitHub Actions](https://github.com/features/actions) action for testing C/C++ projects:

- Use [CMake](https://cmake.org/) to build tests
- Use [Valgrind](https://valgrind.org/) to check for memory leaks
- Use [LCOV](https://github.com/linux-test-project/lcov)/[GCOV](https://gcc.gnu.org/onlinedocs/gcc/Gcov.html) to generate code coverage data and display a report in the log

## Inputs

### `source-path`

Path containing the `CMakeLists.txt` for the tests.

**Default**: `extras/test`

### `build-path`

The top-level directory for build output.

**Default**: `extras/test/build`

### `runtime-paths`

YAML format list of paths to runtime binaries generated by building the tests.

**Default**: `"- extras/test/build/bin/unit-test-binary"`

### `coverage-exclude-paths`

[YAML](https://en.wikipedia.org/wiki/YAML) format list of paths to remove from coverage data.

**Default**:

```yaml
- '*/extras/test/*'
- '/usr/*'
- '*/src/lib/*'
```

### `coverage-data-path`

Path to save the coverage data file to.

**Default**: `extras/test/build/coverage.info`


## Example usage

```yaml
on: [pull_request, push]
jobs:
  test:
    runs-on: ubuntu-latest

    env:
      COVERAGE_DATA_PATH: extras/coverage-data/coverage.info

    steps:
      - uses: actions/checkout@v2

      - uses: arduino/cpp-test-action@main
        with:
          coverage-data-path: ${{ env.COVERAGE_DATA_PATH }}

      # Optional: upload coverage report to codecov.io
      - uses: codecov/codecov-action@v1
        with:
          file: ${{ env.COVERAGE_DATA_PATH }}
```
