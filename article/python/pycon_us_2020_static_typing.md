# PyCon US 2020 - 파이썬 정적 타이핑

[Watch On Youtube](https://www.youtube.com/watch?v=ST33zDM9vOE)

## Pop quiz
Is python dynamically or statically typed?

**Answer:**
Dynamically typed... but can optionally be statically typed.

### Steps to understand that
- Types in Python
- Type systems in general
- Dynamic typing in Python
- Static typing in Python

### Once we understand that
- How to use static typing
- When you should use static typing
- When you *shouldn't* use static typing

## types (and `type`)

### Dynamic typing
- Variables can be *any* type
- Arguments and return values of functions can be *any* type

```python
>>> def frobnicate(a, b, c):
...     return a+b+c
    
>>> frobnicate(1, 2, 3)
6
>>> frobnicate('Hi', ' ', 'There')
'Hi There'
>>> frobnicate(1, 2, 'foo')
Traceback (most recent call last):
  File "<stdin>", line 1, in <module?
  File "<stdin>", line 1, in frobnicate
TypeError: unsupported operand type(s) for +: 'int' and 'str'
```

To solve wrong input type...
- Write docstring
- Assert inputs
- **Duck typing**
  - If it walks like a duck and it quacks like a duck...
  - Determine the types of a function or a variable based on how it's used
  
### Static typing
- As in, defined and not changing

Statically typed languages
- C/C++
- Java
- Rust
- Typescript
- ...

Python? Kinda..

#### PEP 3107
Function Annotations
- Add some metadata to annotate the arguments and the return value of the function

```python
>>> def frobnicate(a: int, b: int, c: int) -> int:
...	    "Frobnicates the bizbaz"
...     return a + b + c
>>> frobnicate.__annotations__
{'a': int, 'b': int, 'c': int, 'return': int}
```

But still this doesn't give us any way to actually evaluate whether this function is being used correctly elsewhere it's still just metadata.

It also doesn't give us a way to annotate variables we can only annotate functions, their arguments, and return types.

#### PEP 483
The Theory of Type Hints
- Ideas on how static typing should work in python

**Optional typing**
- Only gets in your way if you want it to get in your way

**Gradual typing**
- Let's not try to do this all at once

**Variable annotations**
- For annotating more than just functions

**Special type constructs**
- Fundamental building blocks we need to do static typing

![](https://velog.velcdn.com/images/choi-jiwoo/post/8e4bd170-30df-46df-b765-c5ae0818a8d1/image.png)

```python
def frobnicate(a: int, b: int, c: Union[int ,float]) -> Union[int, float]:
	return a+b+c
```

**Container types**
- For defining types inside container classes
- Replaced in PEP 526

```python
users = [] # type: List[int]
users.append('Jack')  # Error
```

**Generic types**
- For when a clas or function behaves in generic manner

```python
from typing import Iterable

class Task:
	pass
    
def work(todo_list: Iterable[Task]) -> None:
	pass
```

**Type aliases**
- To be more succinct

```python
from typing import Union
from decimal import Decimal

Number = Union[int, float, complex, Decimal]  # type alias

def frob(a: Number, b: Number, c: Number) -> Number:
	"Frobnicates the bizbaz"
    return a+b+c
```

#### PEP 484
Type Hints
- It introduces the typing module to provide standard definitions fundamental building blocks and tools

#### PEP 526
Syntax for Variable Annotations

```python
primes: List[int] = []
captain: str = ...
```

### Type Checkers
Static vs. Dynamic

Static type checkers don't actually run your code. They look at it at rest when it's not being evaluated.

Dynamic type checkers would check types while your application is actually running.

![](https://velog.velcdn.com/images/choi-jiwoo/post/3e2a7e9e-6ffd-4148-9079-dc39e6c68701/image.png)

### Difference between `mypy` and `pytype`

- Cross-function inference
- Runtime lenience

#### Cross-function inference
**mypy** doesn't have the ability to infer types across multiple function calls
```python
# example.py
def f():
	return "PyCon"
    
def g():
	return f() + 2020
    
g()
```
Running `example.py`
**mypy** - does not throw an error
**pytype** - throws an error

#### Runtime lenience
Sort of comes down to a difference in philosophy between these two tools. **pytype** is going to allow any operation that will succeed at runtime and doesn't contradict any existing annotations.
```python
# example.py
from typing import List

def f() -> List[str]:
	lst = ["PyCon"]
    lst.append(2020)
    return [str(x) for x in lst]

print(f())
```
Running `example.py`
**mypy** - throws an error
**pytype** - does not throw an error

### When you shouldn't use static typing
- Basically Never
- Static typing is not just a replacement for unit tests

### When you should use static typing
- Basically as much as possible
- When you're millions-of-lines scale

> "At our scale-millions of lines of Python-the dynamic typing in Python made code needlessly hard to understand and started to seriously impact productivity." - Jukka Lehtosalo

- When your code is confusing
- When your code is for public consumption
- Before migrating or refactoring
- To experiment with static typing

### How to use static typing in Python
*In just five easy steps*

1. Migrate to Python >= 3.6
2. Install a typechecker locally
3. Start optionally typing your codebase
4. Run a typechecker with your linting
5. Convince all your coworkers to join you
