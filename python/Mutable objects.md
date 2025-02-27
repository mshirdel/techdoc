In Python, mutable objects are objects whose state or content can be changed after they are created. This means you can modify the contents of a mutable object without creating a new object. Examples of mutable objects include:

### Lists
Lists are a common example of mutable objects. You can change their content, append new elements, or remove existing ones.
```python
my_list = [1, 2, 3]
my_list.append(4)  # List is now [1, 2, 3, 4]
my_list[0] = 0    # List is now [0, 2, 3, 4]
```

### Dictionaries
Dictionaries are also mutable. You can add, remove, or change key-value pairs.
```python
my_dict = {"a": 1, "b": 2}
my_dict["c"] = 3      # Dictionary is now {'a': 1, 'b': 2, 'c': 3}
my_dict["a"] = 0      # Dictionary is now {'a': 0, 'b': 2, 'c': 3}
del my_dict["b"]      # Dictionary is now {'a': 0, 'c': 3}
```

### Sets
Sets are mutable as well. You can add or remove elements from a set.
```python
my_set = {1, 2, 3}
my_set.add(4)    # Set is now {1, 2, 3, 4}
my_set.remove(1) # Set is now {2, 3, 4}
```

### User-defined classes
Objects of user-defined classes can be mutable if the class allows modification of its attributes.
```python
class MyClass:
    def __init__(self, value):
        self.value = value

obj = MyClass(10)
obj.value = 20  # Object's value attribute is now 20
```

### Why Mutability Matters
Mutability is an important concept because it affects how objects are passed around and manipulated within a program. Mutable objects can lead to side effects if they are changed unintentionally, which can cause bugs that are hard to track down. On the other hand, mutability is useful when you need to make incremental changes to an object without creating a new one each time.

---

Using mutable objects as default parameters in functions can be tricky because the default parameter is evaluated only once when the function is defined, not each time the function is called. This means that if you modify the mutable object, it will retain those changes across subsequent calls to the function. This can lead to unexpected behavior.

Here's an example to illustrate the issue:

```python
def append_to_list(value, my_list=[]):
    my_list.append(value)
    return my_list

# First call - works as expected
print(append_to_list(1))  # Output: [1]

# Second call - unexpected behavior
print(append_to_list(2))  # Output: [1, 2]
```

As you can see, the second call to `append_to_list` appends to the same list used in the first call, leading to unintended behavior.

### Recommended Approach
To avoid this issue, you can use `None` as the default value and create a new mutable object inside the function if needed. Here's how:

```python
def append_to_list(value, my_list=None):
    if my_list is None:
        my_list = []
    my_list.append(value)
    return my_list

# Now each call behaves as expected
print(append_to_list(1))  # Output: [1]
print(append_to_list(2))  # Output: [2]
```

In this version, `my_list` is set to a new empty list each time the function is called without a specific `my_list` argument. This ensures that each call operates on a separate list.

Using this approach, you can safely use mutable objects as default parameters in functions without unintended side effects. If you have any more questions or need further examples, feel free to ask!