---
title: Testing
---
# Environment Variables

Use the `unittest.mock` library to overwrite environment variables for tests or test suites.

```python
import os
from unittest import mock


@mock.patch.dict(os.environ, {'SOME_VARIABLE': 'value for testing'})
class TestFoo:
    ...
```

# Mocks

[Official documentation](https://docs.python.org/3/library/unittest.mock.html)

### `Mock` vs `patch`

If you are passing an instance of a class to the unit under test (UUT), e.g. via a constructor argument, then in your tests you can create a new `Mock(spec=SomeClass)` instance and pass that instead.

If your UUT creates its own instances of dependencies rather than accepting them as arguments, then you have to `patch(autospec=True)` the imported class to globally replace the library class with a mock.

### Mocking an imported class

**package1/file1.py**
```python
from somelib import SomeClass

def do_something(myid):
    x = SomeClass(myid=myid)
    x.doit()
```

**test/package1/test_file1.py**
```python
from unittest import mock

import pytest

from package1 import file1
from package1.file1 import do_something


@pytest.fixture
def mock_some_class():
    with mock.patch.object(file1, 'SomeClass', autospec=True) as mock_some_class:
        yield mock_some_class


class TestFile1:
    def test_do_something(self, mock_some_class):
        do_something('someid')
        mock_some_class.assert_called_once_with(myid='someid')
        mock_some_class.return_value.doit.assert_called_once()
```

How to call `mock.patch` and its methods depends on how the library is imported in `file1.py`. Generally you have to patch whatever's present in `file1.py`, *not* the `somelib.SomeClass` class itself. [Further details in the documentation.](https://docs.python.org/3/library/unittest.mock.html#where-to-patch)

### Setting up failures

The simplest way to raise an exception when a mocked method is called is like this:
```python
mock.patch.object(
    file1,
    'SomeClass',
    side_effect=RuntimeError('fail'),
    autospec=True
)
```

Here's a more complicated example in which we conditionally fail calls to the constructor of the mocked class:

**test/package1/test_file1.py**
```python
from unittest import mock
from unittest.mock import call, DEFAULT

import pytest

from package1 import file1
from package1.file1 import do_something


def constructor_side_effect(myid):
    if myid == 'bad':
        raise RuntimeError('bad id')
    return DEFAULT  # ensures that mock_some_class() returns a mock as expected


@pytest.fixture
def mock_some_class():
    with mock.patch.object(
        file1,
        'SomeClass',
        side_effect=constructor_side_effect,
        autospec=True,
    ) as mock_some_class:
        yield mock_some_class


class TestFile1:
    def test_do_something(self, mock_some_class):
        do_something('someid')

        with pytest.raises(RuntimeError):
            do_something('bad')

        expected_calls = [
            call(myid='someid'),
            call(myid='bad'),
        ]
        # use any_order=True becase calls to SomeClass.doit() seem to count too
        mock_some_class.assert_has_calls(expected_calls, any_order=True)
        mock_some_class.return_value.doit.assert_called_once()
```