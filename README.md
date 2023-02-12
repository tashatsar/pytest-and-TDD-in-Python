# Unit testing with `pytest`🐍🚨


## Tests, unit tests, TDD 🍎🍄
### Levels of testing
✨ **Unit testing**: testing fucntions and classes. Done by developers. Smallest unit of testing possible, module or component is tested in isolation. 

✨✨ **Component testing**: testing interfacing issues between the modules. Done by testers.

✨✨✨ **System testing**: testing application as a whole thing. Done by testers. Made in environment similar to the production one.

✨✨✨✨**Acceptance testing**: testing if application satisfies end-user requirements and can be deployed. Can be made by testers as well as final users (alpha and beta testing).  

### What is unit testing?
- Unit testing tests individual modules independently 🍎
- Unit tests are the first filter for catcing bugs 🐛
- Tests should run fast with automated execution 🚀
- Unit testing is for development environment rather than the production one 🔨

### What is test driven development (TDD)?
- Tests are written before the production code
- Tests and production code are written step by step
- The process is iterative: 🚨red pahse, ✅green phase, 🔨refactor phase

🚨write a failing unit test -> ✅write prod code to pass only this test -> 🔨refactor test and code to make it clean -> 🚨write a new failing unit test -> ✅write prod code to pass only the new test -> 🔨refactor test and code to make it clean ->🚨-> ✅->🔨-> etc. until the feature is complete

#### Laws of TDD
Robert Martin created the following Laws of TDD in his book “Clean Code: A Handbook of Agile Software Development”:
1. You may not write any production code until you have written a failing unit test.
2. You may not write more of a unit test than is sufficient to fail, and not compiling is failing.
3. You may not write more production code than is sufficient to pass the currently failing unit test

## `pytest`📦
`pytest` is a [framework for tests](https://docs.pytest.org/en/7.2.x/). It uses standart Python assert statement 🐍
- **tests** are Python functions with "test" in the beginnig of the fucntion name: `test_pipeline_step`, `test_sum`. Tests can be grouped by putting them in the same module or class.
- **test class** should have "Test"  in the beginnig of the class name: `TestClass`, `TestSimilarThings`. Test class should NOT have `__init__` method.
- **filenames** for test modules should have "test" in the beginning or end of the filename: `test_*.py` or `*_test.py`.

BTW, the naming rules described above can be changed in case you need. For doing so create a file named `pytest.ini` in the directory with test. Fill the file with the following content. Let's suppose we want to replace word "test" for test functions only with word "check" (this is just an example):
```
[pytest]
python_files = test_*
python_classes = *Tests
python_functions = check_*
```

### Install `pytest`🧰📦

`pip install pytest`: requires Python 3.7+
Install plugins:
- `pip install pytest-cov`: coverage reporting, produces coverage reports
- `pip install pytest-html`: generates a HTML report for test results
- `pip install pytest-xdist`: allows distributed execution modes (parallel test exevution). Tests have to be isolated, otherwise doesn't make sence :melting_face:

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

Command line for `pytest-html` plugin:
- `python -m pytest --html="results.html"`: generate HTML report what is called "results.html"

Command line for `pytest-html` plugin:
- `python -m pytest -n4`: run test using 4 treads. n3 is for 3 threads, etc. `-nauto` is supported. 

With several arguments: `python -m pytest -s -v`

### `pytest` decorators 🌺📦
- **`@pytest.fixture`**: need to use the same parameter for several tests? 
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

- **`@pytest.mark.parametrize`**: need to use different parameter sin the same test? 
Don't use loops, use parametrization!

```py
@pytest.mark.parametrize("our_params", ["param_value_1", "param_value_2", "param_value_3"])
def test_with_params(our_params):
  assert our_params
```
- **`@pytest.mark`**: need to create groups of tests to be run without other tests? Just mark your tests!
```py 
@pytest.mark.feature_a
def test_1_about_feature_A():
  assert smth

@pytest.mark.feature_a
def test_2_about_feature_A():
  assert smth
  
def test_1_about_feature_B():
  assert smth

```
To launch only test_1_about_feature_A and test_2_about_feature_A run in command line `python -m pytest -m feature_a`. In case you have to run several tests by how they are marked, use `python -m pytest -m "feature_a or feature_b"` (for tests that are marked with at least one of decoratos) and `python -m pytest -m "feature_a or feature_b"`(for tests that are marked with both at the same time). Those code markers can be described in the `pytest.ini` file. It is higly recommended for better code readability. For example:
```
[pytest]
python_files = test_*
python_classes = *Tests
python_functions = test_*

markers =
  feature_a: All tests for feature A testing 
```
 
- **`@pytest.mark.skip`**: need to skip a test? 
Test `test_to_skip` wont't be executed!

```py
def test():
  assert something
  
@pytest.mark.skip  
def test_to_skip():
  assert smth
```

## Some courses 💻📕🚀
- [Elegant Automation Frameworks with Python and Pytest course](https://www.udemy.com/course/elegant-automation-frameworks-with-python-and-pytest/):[certificate](https://udemy-certificate.s3.amazonaws.com/image/UC-76f2eb84-d321-4658-98fe-607e99e0eacd.jpg) 🐌🐌🐌
  - 👍Pro: tutor's language is pretty alive, with nice sence of humour. Explanations are detailed, the course has a lot of  examples of code that are very easy to understand or reuse. I also liked topics covered in the course👌 
  - 👎Cons: Some explanations can seem too long and too detailed, the lecturer can step aside. Sometimes it can seem that there is not ehough or not at all theoretical part, just examples. I used speed 2x, but desired would be around 3x🐌. Last update of the course was June 2020👴
  - ⌚Duration: 6 hours 
- [Unit Testing and Test Driven Development in Python course](https://udemy.com/course/unit-testing-and-tdd-in-python): [certificate](https://udemy-certificate.s3.amazonaws.com/image/UC-7e412d6d-5ac2-41b3-ab39-0936dc665643.jpg)
  - 👍Pro: short course to get into the topic fast with many examples to start using immediately.
  - 👎Cons: sections about mocks and XUnit style are not detailed at all and provided too fast. 🚗💨 Code lacks explanations in general. Sometimes it's hard to follow the logic of the code.💩 ALso the course is pretty old, have not been updated since 2019. 👴
  - ⌚Duration: 2 hours 
