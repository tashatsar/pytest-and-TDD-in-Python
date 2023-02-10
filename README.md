# Unit testing with `pytest`🐍🚨

## Levels of testing
✨ **Unit testing**: testing fucntions and classes. Done by developers. Smallest unit of testing possible, module or component is tested in isolation. 

✨✨ **Component testing**: testing interfacing issues between the modules. Done by testers.

✨✨✨ **System testing**: testing application as a whole thing. Done by testers. Made in environment similar to the production one.

✨✨✨✨**Acceptance testing**: testing if application satisfies end-user requirements and can be deployed. Can be made by testers as well as final users (alpha and beta testing).  

## What is unit testing?
- Unit testing tests individual modules independently 🍎
- Unit tests are the first filter for catcing bugs 🐛
- Tests should run fast with automated execution 🚀
- Unit testing is for development environment rather than the production one 🔨

## What is test driven development (TDD)?
- Tests are written before the production code
- Tests and production code are written step by step
- The process is iterative: 🚨red pahse, ✅green phase, 🔨refactor phase

🚨write a failing unit test -> ✅write prod code to pass only this test -> 🔨refactor test and code to make it clean -> 🚨write a new failing unit test -> ✅write prod code to pass only the new test -> 🔨refactor test and code to make it clean ->🚨-> ✅->🔨-> etc. until the feature is complete

## Laws of TDD
Robert Martin created the following Laws of TDD in his book “Clean Code: A Handbook of Agile Software Development”:
1. You may not write any production code until you have written a failing unit test.
2. You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
3. You may not write more production code than is sufficient to pass the currently failing unit test

## `pytest`📦
`pytest` is a [framework for tests](https://docs.pytest.org/en/7.2.x/). It uses standart Python assert statement 🐍
- **tests** are Python functions with "test" in the beginnig of the fucntion name: `test_pipeline_step`, `test_sum`. Tests can be grouped by putting them in the same module or class.
- **test class** should have "Test"  in the beginnig of the class name: `TestClass`, `TestSimilarThings`. Test class should NOT have `__init__` method.
- **filenames** for test modules should have "test" in the beginning or end of the filename: `test_*.py` or `*_test.py`.

### Install `pytest`

`pip install pytest`: requires Python 3.7+
Install plugin:
`pip install pytest-cov`: coverage reporting, compatible with distributed testing

### Run `pytest` with command line arguments
Run from the directory with tests: 
`python -m pytest`
for running all tests in all modules from the current working directory (pay attention to [naming](#pytest)). With command line arguments (all params [here](https://docs.pytest.org/en/7.1.x/reference/reference.html#command-line-flags)):
- `python -m pytest -v`: verbose mode
- `python -m pytest -q`: quiet mode (when running hundreds of tests)
- `python -m pytest -s`: show print statements on the console, don't capture the console output

Command line arguments for `pytest-cov` plugin (more params [here](https://pytest-cov.readthedocs.io/en/latest/config.html)): 
- `python -m pytest --cov-report=term`: generate coverage report in terminal
- `python -m pytest --cov-report=html`: generate coverage report as HTML

With several arguments: `python -m pytest -s -v`

### `pytest` decorators

#### `@pytest.fixture`: need to use the same parameter for several tests? 
Fixture will be executed before the test!
```py
@pytest.fixture()
def import_package():
  import packages.package as package 
  return package
  
def test_module_1_exsists(import_package):
  assert (hasattr(import_package, 'module_1'))
  
def test_module_2_exsists(import_package):
  assert (hasattr(import_package, 'module_2'))
  
def test_no_fixture():
  assert smth
```

#### `@pytest.mark.parametrize`: need to use different parameter sin the same test? 
Don't use loops, use parametrization!

```py
@pytest.mark.parametrize("our_params", ["param_value_1", "param_value_2", "param_value_3"])
def test_with_params(our_params):
  assert our_params
```
 
#### `@pytest.mark.skip`: need to skip a test? 
Test `test_to_skip` wont't be executed!

```py
def test():
  assert something
  
@pytest.mark.skip  
def test_to_skip():
  assert smth
```


<!-- can create instance of a class -->




[Unit Testing and Test Driven Development in Python course](https://udemy.com/course/unit-testing-and-tdd-in-python) 🚨✅✔️
