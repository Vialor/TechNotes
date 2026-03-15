> `pytest`

Arrange-Act-Assert model:
1. **Arrange**, or set up, the conditions for the test
2. **Act** by calling some function or method
3. **Assert** that some end condition is true
## Arrange: 
### `@pytest.fixture(scope)`
The `scope` parameter in fixture defines how often the fixture is invoked and how long its state is shared among tests. The possible values for `scope` are:
- **"function"** (default): The fixture is invoked once per test function.
- **"class"**: The fixture is invoked once per test class. The fixture state is shared among all test methods in the class.
- **"module"**: The fixture is invoked once per test module. The fixture state is shared among all tests in the module.
- **"package"**: The fixture is invoked once per test package (a directory containing an `__init__.py` file). The fixture state is shared among all tests in the package.
- **"session"**: The fixture is invoked once per test session. The fixture state is shared among all tests in the session.
### `@pytest.mark.parametrize()`
```python
@pytest.mark.parametrize('test_type, var1, var2, expect', [  
	('verify_max_returned', 'apple', 90, 100),  
	('verify_min_returned', 'orange', 2, 10)  
])  
def test_get_size(mocker, test_type, var1, var2, expect):  
	print("Logging test type for visibility: " + test_type)  
	assert fleet_manager.get_optimal_size('zone1', var1, var2) == expect
```
### Mock: `pytest-mock`
```python
# mock constants
def test_mocking_constant_a(mocker):
    mocker.patch.object(mock_examples.functions, 'CONSTANT_A', 2)
    # OR: mocker.patch('mock_examples.functions.CONSTANT_A', 2)
    actual = double()  # CONSTANT_A is used in double function
	assert actual == 4

# mock functions
def test_slow_function_mocked_api_call(mocker):
    mocker.patch(
        'mock_examples.main.api_call',
        return_value=5
    )
    # OR: api_mock = mocker.patch('mock_examples.main.api_call')
    #     api_mock.return_value = 5
    actual = slow_function() # api_call is a function used in slow_function
    assert actual == 5

# mock object methods
def test_mocking_class_method(mocker):
    def mock_load(self):
        return 'xyz'
    mocker.patch(
        'mock_examples.main.Dataset.load_data',
        mock_load
    )
    actual = slow_dataset() # Dataset.load_data is used in slow_dataset
    assert actual == 'xyz'
```