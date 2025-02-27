Certainly! Here's an advanced Python interview question that touches on several key concepts:

### Question:
**Implement a custom iterator class in Python that mimics the behavior of the built-in `range` function but also supports floating-point numbers. Include methods for iteration and the ability to check the length of the iterator.**

### Requirements:
1. The class should be named `FloatRange`.
2. It should accept three parameters: `start`, `stop`, and `step`, where `start` and `step` can be floating-point numbers.
3. Implement the `__iter__()` and `__next__()` methods to make the class iterable.
4. Include a method named `__len__()` that returns the number of steps in the range.

### Example Usage:
```python
fr = FloatRange(1.5, 5.5, 0.5)
for num in fr:
    print(num)

print(len(fr))  # Output: Number of steps in the range
```

### Solution:
```python
class FloatRange:
    def __init__(self, start, stop, step):
        self.start = start
        self.stop = stop
        self.step = step
        self._current = start

    def __iter__(self):
        self._current = self.start
        return self

    def __next__(self):
        if self._current >= self.stop:
            raise StopIteration
        current = self._current
        self._current += self.step
        return current

    def __len__(self):
        return max(0, int((self.stop - self.start) // self.step))

# Example usage
fr = FloatRange(1.5, 5.5, 0.5)
for num in fr:
    print(num)

print(len(fr))  # Output: 8
```

### Explanation:
- **`__init__()`**: Initializes the iterator with `start`, `stop`, and `step` values and sets the initial `_current` value.
- **`__iter__()`**: Resets the `_current` value to `start` and returns the iterator object itself.
- **`__next__()`**: Implements the logic for iteration. It raises a `StopIteration` exception when the end of the range is reached.
- **`__len__()`**: Calculates the number of steps in the range.

This question tests the candidate's understanding of iterators, custom class implementation, and handling floating-point arithmetic. 

Feel free to use or modify this question for your interview process! If you have any other specific topics in mind, let me know!

---

Certainly! Here’s a more advanced Python interview question that encompasses multiple concepts such as decorators, metaclasses, and context managers:

### Question:
**Design a system in Python that allows for timed execution of methods within classes. Specifically, create a metaclass that applies a decorator to all methods of a class, timing their execution and logging the duration. Also, ensure that this system can be used with a context manager to enable and disable timing dynamically.**

### Requirements:
1. **Metaclass**: Create a metaclass named `TimedMeta` that applies a timing decorator to all methods of a class.
2. **Decorator**: Implement a decorator that logs the execution time of a method.
3. **Context Manager**: Implement a context manager to enable and disable the timing dynamically.
4. **Example Usage**: Demonstrate the functionality with a sample class.

### Example Usage:
```python
class MyClass(metaclass=TimedMeta):
    def method1(self):
        # Simulate some work
        time.sleep(1)
        print("Method 1 executed.")

    def method2(self):
        # Simulate some work
        time.sleep(2)
        print("Method 2 executed.")

with TimingContextManager(enabled=True):
    obj = MyClass()
    obj.method1()
    obj.method2()

# Output should include the execution time of each method.
```

### Solution:

```python
import time
from functools import wraps

# Timing decorator
def timing_decorator(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start_time = time.time()
        result = func(*args, **kwargs)
        end_time = time.time()
        print(f"{func.__name__} took {end_time - start_time:.4f} seconds to execute.")
        return result
    return wrapper

# Metaclass to apply the decorator
class TimedMeta(type):
    def __new__(cls, name, bases, dct):
        for attr, value in dct.items():
            if callable(value):
                dct[attr] = timing_decorator(value)
        return super().__new__(cls, name, bases, dct)

# Context manager to enable/disable timing
class TimingContextManager:
    def __init__(self, enabled=True):
        self.enabled = enabled

    def __enter__(self):
        global timing_decorator
        if not self.enabled:
            timing_decorator = lambda func: func

    def __exit__(self, exc_type, exc_val, exc_tb):
        global timing_decorator
        # Reset timing_decorator to the original one after the context
        timing_decorator = self._original_timing_decorator

    # Save the original timing_decorator to reset later
    _original_timing_decorator = timing_decorator

# Example usage
class MyClass(metaclass=TimedMeta):
    def method1(self):
        time.sleep(1)
        print("Method 1 executed.")

    def method2(self):
        time.sleep(2)
        print("Method 2 executed.")

with TimingContextManager(enabled=True):
    obj = MyClass()
    obj.method1()
    obj.method2()
```

### Explanation:
1. **`timing_decorator`**: A decorator function that measures and logs the execution time of a method.
2. **`TimedMeta`**: A metaclass that applies the `timing_decorator` to all methods of a class.
3. **`TimingContextManager`**: A context manager that enables and disables the timing dynamically by temporarily replacing the `timing_decorator`.

This question tests the candidate's understanding of metaclasses, decorators, and context managers, as well as their ability to combine these concepts to create a flexible and powerful system.

Feel free to use or tweak this question for your interview process. If you have any other specific topics in mind or need further examples, let me know!