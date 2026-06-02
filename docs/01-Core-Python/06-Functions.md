# Python Functions

> Reusable logic, parameters, return values, scope, closures, validation, clean code, and interview-ready problem solving.

---

# Why Functions Matter

Functions are one of the most important building blocks in Python.

A function allows you to:

* Reuse logic
* Organize code
* Reduce duplication
* Test behavior independently
* Separate responsibilities
* Build clean APIs
* Compose larger systems from smaller units

In interviews, functions are used to test whether you can:

* Structure code clearly
* Handle inputs and outputs
* Avoid side effects
* Use default arguments safely
* Understand scope
* Understand closures
* Write reusable utilities
* Design clean production logic

For Senior Software Engineer and AI Engineer roles, functions appear everywhere:

* FastAPI endpoints
* ML preprocessing pipelines
* Validation utilities
* Retry wrappers
* Embedding functions
* RAG retrieval logic
* Model evaluation helpers
* Data transformation steps
* Background jobs
* Service layer methods

A strong Python engineer must write functions that are clear, predictable, testable, and maintainable.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Define and call functions
* Use parameters and return values
* Explain positional and keyword arguments
* Use default arguments safely
* Explain `*args` and `**kwargs`
* Explain local, global, and nonlocal scope
* Understand first-class functions
* Use functions as arguments
* Return functions from functions
* Explain closures
* Use lambda functions appropriately
* Write pure functions
* Avoid dangerous side effects
* Add type hints and docstrings
* Design production-ready utility functions
* Solve common function-based interview questions

---

# What Is A Function?

A function is a reusable block of code that performs a specific task.

Example:

```python
def greet(name):
    return f"Hello, {name}"
```

Call the function:

```python
message = greet("Abubaker")

print(message)
```

Output:

```python
Hello, Abubaker
```

---

# Why Use Functions?

Without functions:

```python
name = "Abubaker"
print(f"Hello, {name}")

name = "Sara"
print(f"Hello, {name}")

name = "Ali"
print(f"Hello, {name}")
```

With a function:

```python
def greet(name):
    print(f"Hello, {name}")

greet("Abubaker")
greet("Sara")
greet("Ali")
```

Benefits:

* Less duplication
* Easier testing
* Easier debugging
* More readable code
* Better separation of concerns

---

# Basic Function Syntax

```python
def function_name(parameter):
    result = parameter * 2
    return result
```

Parts:

```text
def              -> function definition keyword
function_name    -> function name
parameter        -> input
return           -> output
```

Example:

```python
def double(x):
    return x * 2
```

Call:

```python
double(5)
```

Output:

```python
10
```

---

# Parameters vs Arguments

These terms are often confused.

## Parameter

A parameter is the variable defined in the function signature.

```python
def greet(name):
    return f"Hello, {name}"
```

Here:

```text
name
```

is a parameter.

## Argument

An argument is the actual value passed to the function.

```python
greet("Abubaker")
```

Here:

```text
"Abubaker"
```

is an argument.

---

# Return Values

A function can return a result.

```python
def add(a, b):
    return a + b
```

Usage:

```python
result = add(3, 4)

print(result)
```

Output:

```python
7
```

If a function has no explicit return, Python returns `None`.

```python
def say_hi():
    print("Hi")

result = say_hi()

print(result)
```

Output:

```python
Hi
None
```

---

# Multiple Return Values

Python functions can return multiple values.

```python
def min_max(nums):
    return min(nums), max(nums)
```

Usage:

```python
low, high = min_max([3, 1, 9, 2])
```

Behind the scenes, Python returns a tuple.

```python
result = min_max([3, 1, 9, 2])

print(result)
```

Output:

```python
(1, 9)
```

This connects directly to the previous Tuples chapter.

---

# Real ML Example: Return Metrics

```python
def evaluate_predictions(y_true, y_pred):
    accuracy = calculate_accuracy(y_true, y_pred)
    precision = calculate_precision(y_true, y_pred)
    recall = calculate_recall(y_true, y_pred)

    return accuracy, precision, recall
```

Usage:

```python
accuracy, precision, recall = evaluate_predictions(y_true, y_pred)
```

This pattern is common in ML coding interviews.

---

# Positional Arguments

Arguments can be passed by position.

```python
def create_user(name, role, country):
    return {
        "name": name,
        "role": role,
        "country": country
    }

user = create_user("Abubaker", "Software Engineer", "UAE")
```

Order matters.

Wrong order can create bugs:

```python
user = create_user("UAE", "Abubaker", "Software Engineer")
```

This is valid Python but logically wrong.

---

# Keyword Arguments

Keyword arguments make calls clearer.

```python
user = create_user(
    name="Abubaker",
    role="Software Engineer",
    country="UAE"
)
```

Benefits:

* More readable
* Less risk of wrong order
* Better for functions with many parameters

---

# Default Arguments

You can provide default values.

```python
def greet(name, title="Mr"):
    return f"Hello {title}. {name}"
```

Usage:

```python
greet("Abubaker")
```

Output:

```python
Hello Mr. Abubaker
```

Override default:

```python
greet("Sara", title="Dr")
```

Output:

```python
Hello Dr. Sara
```

---

# Dangerous Mutable Default Arguments

This is one of the most common Python interview traps.

Bad:

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

Call:

```python
print(add_item("a"))
print(add_item("b"))
print(add_item("c"))
```

Output:

```python
['a']
['a', 'b']
['a', 'b', 'c']
```

Why?

Default arguments are evaluated once when the function is defined.

The same list is reused across calls.

---

# Correct Pattern

Use `None` as the default.

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

Now:

```python
print(add_item("a"))
print(add_item("b"))
print(add_item("c"))
```

Output:

```python
['a']
['b']
['c']
```

This pattern is extremely important in production Python.

---

# *args

`*args` allows a function to accept any number of positional arguments.

```python
def add_all(*numbers):
    total = 0

    for number in numbers:
        total += number

    return total
```

Usage:

```python
add_all(1, 2, 3, 4)
```

Output:

```python
10
```

Inside the function, `numbers` is a tuple.

```python
def show_args(*args):
    print(args)
    print(type(args))

show_args(1, 2, 3)
```

Output:

```python
(1, 2, 3)
<class 'tuple'>
```

---

# When To Use *args

Use `*args` when:

* Number of positional inputs is flexible
* You are writing utility functions
* You are wrapping another function
* You are building flexible APIs

Example:

```python
def average(*values):
    if not values:
        return 0

    return sum(values) / len(values)
```

---

# **kwargs

`**kwargs` allows a function to accept any number of keyword arguments.

```python
def create_profile(**kwargs):
    return kwargs
```

Usage:

```python
profile = create_profile(
    name="Abubaker",
    role="Software Engineer",
    country="UAE"
)

print(profile)
```

Output:

```python
{
    'name': 'Abubaker',
    'role': 'Software Engineer',
    'country': 'UAE'
}
```

Inside the function, `kwargs` is a dictionary.

```python
def show_kwargs(**kwargs):
    print(kwargs)
    print(type(kwargs))

show_kwargs(name="Abubaker", role="Engineer")
```

Output:

```python
{'name': 'Abubaker', 'role': 'Engineer'}
<class 'dict'>
```

---

# When To Use **kwargs

Use `**kwargs` when:

* You need flexible named parameters
* You are forwarding options
* You are building wrappers
* You are accepting configuration

Example:

```python
def build_model_config(**options):
    config = {
        "temperature": 0.2,
        "max_tokens": 1000,
        "timeout": 30
    }

    config.update(options)

    return config
```

Usage:

```python
config = build_model_config(
    temperature=0.7,
    timeout=60
)
```

---

# Combining Normal Args, *args, and **kwargs

Order matters.

Correct order:

```python
def function_name(required, default=None, *args, **kwargs):
    pass
```

Example:

```python
def log_event(event_type, *args, **metadata):
    print("Event:", event_type)
    print("Args:", args)
    print("Metadata:", metadata)
```

Call:

```python
log_event(
    "invoice.created",
    "urgent",
    user_id="u123",
    source="api"
)
```

Output:

```text
Event: invoice.created
Args: ('urgent',)
Metadata: {'user_id': 'u123', 'source': 'api'}
```

---

# Unpacking Arguments

You can unpack a list or tuple into function arguments.

```python
def add(a, b, c):
    return a + b + c

values = [1, 2, 3]

result = add(*values)
```

Output:

```python
6
```

You can unpack a dictionary into keyword arguments.

```python
def create_user(name, role, country):
    return {
        "name": name,
        "role": role,
        "country": country
    }

data = {
    "name": "Abubaker",
    "role": "Software Engineer",
    "country": "UAE"
}

user = create_user(**data)
```

---

# Scope

Scope determines where a variable can be accessed.

Main types:

* Local scope
* Enclosing scope
* Global scope
* Built-in scope

This is often called the LEGB rule.

```text
L -> Local
E -> Enclosing
G -> Global
B -> Built-in
```

---

# Local Scope

Variables created inside a function are local.

```python
def calculate():
    result = 10
    return result

print(result)
```

This raises:

```python
NameError
```

because `result` exists only inside the function.

---

# Global Scope

Variables created outside functions are global.

```python
counter = 0

def show_counter():
    print(counter)

show_counter()
```

Output:

```python
0
```

Reading global variables is allowed.

Modifying them requires care.

---

# global Keyword

```python
counter = 0

def increment():
    global counter
    counter += 1

increment()

print(counter)
```

Output:

```python
1
```

Use `global` carefully.

In production, too much global mutable state causes bugs.

---

# nonlocal Keyword

`nonlocal` is used for variables in an enclosing function.

```python
def outer():
    count = 0

    def inner():
        nonlocal count
        count += 1
        return count

    return inner
```

Usage:

```python
counter = outer()

print(counter())
print(counter())
```

Output:

```python
1
2
```

This leads directly to closures.

---

# First-Class Functions

In Python, functions are first-class objects.

That means functions can be:

* Assigned to variables
* Passed as arguments
* Returned from other functions
* Stored in data structures

Example:

```python
def greet(name):
    return f"Hello, {name}"

say_hello = greet

print(say_hello("Abubaker"))
```

Output:

```python
Hello, Abubaker
```

---

# Passing Functions As Arguments

```python
def apply_operation(x, operation):
    return operation(x)

def square(x):
    return x * x

result = apply_operation(5, square)
```

Output:

```python
25
```

This is the foundation of higher-order functions.

---

# Real Backend Example: Handler Mapping

```python
def handle_invoice_created(event):
    return "invoice handled"

def handle_payment_completed(event):
    return "payment handled"

handlers = {
    "invoice.created": handle_invoice_created,
    "payment.completed": handle_payment_completed
}

event = {
    "type": "invoice.created"
}

handler = handlers[event["type"]]
result = handler(event)
```

This avoids long `if/elif` chains.

---

# Returning Functions

A function can return another function.

```python
def make_multiplier(factor):
    def multiply(value):
        return value * factor

    return multiply
```

Usage:

```python
double = make_multiplier(2)
triple = make_multiplier(3)

print(double(10))
print(triple(10))
```

Output:

```python
20
30
```

---

# Closures

A closure is a function that remembers variables from its enclosing scope.

In the previous example:

```python
def make_multiplier(factor):
    def multiply(value):
        return value * factor

    return multiply
```

`multiply()` remembers `factor`.

Even after `make_multiplier()` finishes.

This is a closure.

Closures are used in:

* Decorators
* Function factories
* Configuration wrappers
* Retry utilities
* Logging wrappers

---

# Lambda Functions

A lambda is a small anonymous function.

```python
square = lambda x: x * x
```

Equivalent to:

```python
def square(x):
    return x * x
```

Common use:

```python
users = [
    {"name": "Ali", "age": 30},
    {"name": "Sara", "age": 25}
]

users.sort(key=lambda user: user["age"])
```

---

# When To Use Lambda

Use lambda when:

* Logic is short
* Function is used once
* It improves readability

Avoid lambda when:

* Logic is complex
* You need multiple lines
* A named function would be clearer

Bad:

```python
process = lambda x: complicated_logic(x)
```

Better:

```python
def process_user(user):
    ...
```

---

# Pure Functions

A pure function:

* Same input always gives same output
* Does not modify external state
* Has no side effects

Example:

```python
def add(a, b):
    return a + b
```

This is pure.

Impure example:

```python
total = 0

def add_to_total(value):
    global total
    total += value
    return total
```

This depends on external state.

Pure functions are easier to:

* Test
* Debug
* Reuse
* Parallelize
* Reason about

---

# Side Effects

A side effect happens when a function changes something outside itself.

Examples:

* Modifying global variables
* Writing files
* Updating databases
* Making API calls
* Printing
* Mutating input arguments

Example:

```python
def add_item(items, item):
    items.append(item)
```

This mutates the input list.

Sometimes side effects are necessary, but they should be intentional.

---

# Safer Alternative: Return New Data

Instead of:

```python
def add_item(items, item):
    items.append(item)
    return items
```

Use:

```python
def add_item(items, item):
    return items + [item]
```

Tradeoff:

* Safer immutability
* But creates a new list
* More memory usage

Senior engineers explain both correctness and performance tradeoffs.

---

# Type Hints

Type hints improve readability and tooling.

```python
def add(a: int, b: int) -> int:
    return a + b
```

Example:

```python
def create_user(name: str, role: str, active: bool = True) -> dict:
    return {
        "name": name,
        "role": role,
        "active": active
    }
```

Better:

```python
from typing import Any

def create_user(name: str, role: str, active: bool = True) -> dict[str, Any]:
    return {
        "name": name,
        "role": role,
        "active": active
    }
```

---

# Docstrings

Docstrings explain what a function does.

```python
def normalize_text(text: str) -> str:
    """
    Normalize text by trimming whitespace and converting to lowercase.
    """
    return text.strip().lower()
```

Good docstrings explain:

* Purpose
* Parameters
* Return value
* Important behavior
* Edge cases

---

# Input Validation

Production functions should validate inputs when needed.

Bad:

```python
def average(nums):
    return sum(nums) / len(nums)
```

Problem:

```python
average([])
```

raises:

```python
ZeroDivisionError
```

Better:

```python
def average(nums: list[float]) -> float:
    if not nums:
        raise ValueError("nums must not be empty")

    return sum(nums) / len(nums)
```

---

# Error Handling

A function should fail clearly.

Bad:

```python
def get_user(users, user_id):
    return users[user_id]
```

If missing, raises raw `KeyError`.

Better depending on context:

```python
def get_user(users, user_id):
    if user_id not in users:
        raise ValueError(f"User not found: {user_id}")

    return users[user_id]
```

Or:

```python
def get_user(users, user_id):
    return users.get(user_id)
```

The correct choice depends on whether missing user is expected.

---

# Production AI Example: Text Normalization Function

```python
def normalize_text(text: str) -> str:
    """
    Normalize text before embedding or deduplication.
    """
    return " ".join(
        text.strip().lower().split()
    )
```

Usage:

```python
normalized = normalize_text("  Python   Functions  ")
```

Output:

```python
"python functions"
```

This kind of function is common in RAG pipelines.

---

# Production AI Example: Embedding Cache Key Function

```python
import hashlib

def build_embedding_cache_key(
    model_name: str,
    embedding_version: str,
    text: str
) -> tuple[str, str, str]:
    normalized = normalize_text(text)

    text_hash = hashlib.sha256(
        normalized.encode("utf-8")
    ).hexdigest()

    return model_name, embedding_version, text_hash
```

Why this is good:

* Clear inputs
* Clear output
* Reusable
* Testable
* Avoids repeated hashing logic
* Prevents mixing model versions

---

# Production Backend Example: Retry Function

```python
import time
from collections.abc import Callable
from typing import TypeVar

T = TypeVar("T")

def retry(
    func: Callable[[], T],
    attempts: int = 3,
    delay_seconds: float = 1.0
) -> T:
    last_error = None

    for _ in range(attempts):
        try:
            return func()
        except Exception as error:
            last_error = error
            time.sleep(delay_seconds)

    raise RuntimeError("Function failed after retries") from last_error
```

Usage:

```python
result = retry(lambda: call_external_api())
```

This is a basic version.

In production, you would usually add:

* Exponential backoff
* Jitter
* Logging
* Error filtering
* Timeout handling

---

# Common Mistakes

## Mistake 1: Mutable Default Argument

Bad:

```python
def collect(item, items=[]):
    items.append(item)
    return items
```

Correct:

```python
def collect(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

---

## Mistake 2: Too Many Parameters

Bad:

```python
def create_user(name, age, country, role, salary, department, manager, active):
    ...
```

Better:

* Use a dataclass
* Use Pydantic model
* Use dictionary with validation
* Group related values

---

## Mistake 3: Hidden Side Effects

Bad:

```python
def process_users(users):
    users.clear()
```

This unexpectedly mutates the input.

Better:

```python
def process_users(users):
    processed = []

    for user in users:
        processed.append(transform(user))

    return processed
```

---

## Mistake 4: Returning Different Types

Bad:

```python
def find_user(user_id):
    if user_id == "admin":
        return {"name": "Admin"}

    return False
```

This function returns either:

```text
dict
bool
```

Better:

```python
def find_user(user_id):
    if user_id == "admin":
        return {"name": "Admin"}

    return None
```

Or raise an exception if missing should be exceptional.

---

## Mistake 5: Catching All Exceptions Silently

Bad:

```python
def load_config():
    try:
        ...
    except Exception:
        return {}
```

This hides real bugs.

Better:

```python
def load_config():
    try:
        ...
    except FileNotFoundError:
        return {}
```

Catch only expected exceptions.

---

# Senior Deep Dive: Function Design

A good function should usually:

* Do one thing
* Have a clear name
* Have clear inputs
* Have a clear output
* Avoid surprising side effects
* Be easy to test
* Be small enough to understand
* Fail clearly

Bad:

```python
def process(data):
    ...
```

Better:

```python
def normalize_invoice_payload(payload):
    ...
```

Specific names reduce cognitive load.

---

# Senior Deep Dive: Command Query Separation

A useful design principle:

```text
A function should either return data or change state, but preferably not both.
```

Query function:

```python
def get_user_name(user):
    return user["name"]
```

Command function:

```python
def save_user(user):
    database.save(user)
```

Avoid mixing too much:

```python
def save_user_and_return_report(user):
    ...
```

Sometimes practical systems need combined behavior, but the principle helps maintain clarity.

---

# Senior Deep Dive: Testability

A function is easy to test when it:

* Does not depend on global state
* Does not call external services directly
* Accepts dependencies as parameters
* Returns predictable output

Hard to test:

```python
def get_embedding(text):
    return openai_client.embed(text)
```

Easier to test:

```python
def get_embedding(text, embedding_client):
    return embedding_client.embed(text)
```

This supports dependency injection.

---

# Senior Deep Dive: Function Boundaries In AI Systems

In AI systems, avoid large functions like:

```python
def rag_pipeline(query):
    ...
```

that do everything.

Better:

```python
def rewrite_query(query):
    ...

def retrieve_documents(query):
    ...

def rerank_documents(query, documents):
    ...

def build_prompt(query, documents):
    ...

def generate_answer(prompt):
    ...
```

Benefits:

* Easier testing
* Easier debugging
* Easier observability
* Easier replacement of components
* Better production control

---

# Interview Questions And Answers

## Q1. What is a function?

A function is a reusable block of code that performs a specific task.

It can accept inputs through parameters and return outputs using `return`.

---

## Q2. What is the difference between parameter and argument?

A parameter is the variable defined in the function signature.

An argument is the actual value passed when calling the function.

Example:

```python
def greet(name):
    return f"Hello, {name}"

greet("Abubaker")
```

`name` is the parameter.

`"Abubaker"` is the argument.

---

## Q3. What happens if a function does not return anything?

Python returns `None`.

Example:

```python
def log_message():
    print("Hello")
```

Calling:

```python
result = log_message()
```

sets:

```python
result = None
```

---

## Q4. What are default arguments?

Default arguments provide values when no argument is passed.

```python
def greet(name, title="Mr"):
    return f"Hello {title}. {name}"
```

---

## Q5. Why are mutable default arguments dangerous?

Default arguments are evaluated once when the function is defined.

So this is dangerous:

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

The same list is reused across calls.

Correct:

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

---

## Q6. What is `*args`?

`*args` collects extra positional arguments into a tuple.

```python
def add_all(*numbers):
    return sum(numbers)
```

---

## Q7. What is `**kwargs`?

`**kwargs` collects extra keyword arguments into a dictionary.

```python
def create_profile(**kwargs):
    return kwargs
```

---

## Q8. What is a lambda function?

A lambda is a small anonymous function.

```python
square = lambda x: x * x
```

Use it for short one-line logic, often as a key function in sorting.

---

## Q9. What is a closure?

A closure is a function that remembers variables from its enclosing scope.

Example:

```python
def make_multiplier(factor):
    def multiply(value):
        return value * factor

    return multiply
```

`multiply()` remembers `factor`.

---

## Q10. What is a pure function?

A pure function returns the same output for the same input and has no side effects.

Example:

```python
def add(a, b):
    return a + b
```

Pure functions are easier to test and reason about.

---

## Q11. What is scope?

Scope determines where a variable can be accessed.

Python follows the LEGB rule:

```text
Local
Enclosing
Global
Built-in
```

---

## Q12. What is the difference between `global` and `nonlocal`?

`global` refers to a variable in the module-level scope.

`nonlocal` refers to a variable in an enclosing function scope.

---

# Senior-Level Questions And Answers

## Senior Q1. How do you design a good function?

A good function should:

* Do one thing
* Have a clear name
* Accept clear inputs
* Return clear outputs
* Avoid hidden side effects
* Be easy to test
* Fail clearly
* Avoid unnecessary global state

---

## Senior Q2. Why are pure functions valuable in production systems?

Pure functions are valuable because they are:

* Easy to test
* Easy to debug
* Easy to cache
* Easy to parallelize
* Predictable

In data pipelines and AI systems, pure transformation functions make debugging much easier.

---

## Senior Q3. When should you avoid `**kwargs`?

Avoid `**kwargs` when it hides the function contract.

Bad:

```python
def create_user(**kwargs):
    ...
```

Callers do not know required fields.

Better:

```python
def create_user(name: str, role: str, country: str):
    ...
```

Use `**kwargs` for flexible wrappers or configuration, not as a replacement for clear APIs.

---

## Senior Q4. How would you make a function testable if it calls an external API?

Inject the dependency.

Instead of:

```python
def get_embedding(text):
    return openai_client.embed(text)
```

Use:

```python
def get_embedding(text, embedding_client):
    return embedding_client.embed(text)
```

Now tests can pass a fake client.

---

## Senior Q5. What is a common problem with large functions?

Large functions often mix multiple responsibilities.

They are difficult to:

* Test
* Debug
* Reuse
* Profile
* Modify safely

Break them into smaller focused functions.

---

## Senior Q6. How would you structure a RAG pipeline with functions?

Break it into clear stages:

```python
def normalize_query(query):
    ...

def retrieve_documents(query):
    ...

def rerank_documents(query, documents):
    ...

def build_prompt(query, documents):
    ...

def generate_answer(prompt):
    ...
```

Each function should have one clear responsibility.

---

## Senior Q7. What is the risk of hidden side effects?

Hidden side effects make behavior unpredictable.

Example:

```python
def process(items):
    items.clear()
```

A caller may not expect the input list to be modified.

This can cause production bugs.

---

# Exercises

## Exercise 1 — Basic Function

Write a function:

```python
def greet(name):
    pass
```

Return:

```text
Hello, <name>
```

---

## Exercise 2 — Add Numbers

Write:

```python
def add(a, b):
    pass
```

---

## Exercise 3 — Return Min And Max

Write:

```python
def min_max(nums):
    pass
```

Return both minimum and maximum.

---

## Exercise 4 — Safe Average

Write:

```python
def average(nums):
    pass
```

If the list is empty, raise `ValueError`.

---

## Exercise 5 — Use Default Argument

Write:

```python
def create_user(name, role="user"):
    pass
```

Return a dictionary.

---

## Exercise 6 — Fix Mutable Default

Fix:

```python
def add_tag(tag, tags=[]):
    tags.append(tag)
    return tags
```

---

## Exercise 7 — Use *args

Write:

```python
def multiply_all(*numbers):
    pass
```

Return product of all numbers.

---

## Exercise 8 — Use **kwargs

Write:

```python
def build_config(**options):
    pass
```

Start with defaults:

```python
{
    "timeout": 30,
    "retries": 3
}
```

Override with `options`.

---

## Exercise 9 — Higher-Order Function

Write:

```python
def apply_to_list(items, func):
    pass
```

Apply `func` to every item.

---

## Exercise 10 — Closure

Write:

```python
def make_prefixer(prefix):
    pass
```

It should return a function that adds the prefix to text.

---

## Exercise 11 — Lambda Sorting

Given:

```python
users = [
    {"name": "Ali", "age": 30},
    {"name": "Sara", "age": 25},
    {"name": "Omar", "age": 35}
]
```

Sort by age.

---

## Exercise 12 — Normalize Text

Write:

```python
def normalize_text(text):
    pass
```

It should:

* trim spaces
* lowercase text
* collapse repeated spaces

---

## Exercise 13 — Build Embedding Cache Key

Write:

```python
def build_embedding_cache_key(model_name, version, text):
    pass
```

Return a tuple:

```text
(model_name, version, text_hash)
```

---

## Exercise 14 — Validate Email

Write:

```python
def is_valid_email(email):
    pass
```

Simple validation:

* contains `@`
* domain contains `.`

---

## Exercise 15 — Retry Wrapper

Write a simple retry function:

```python
def retry(func, attempts=3):
    pass
```

---

# Solutions

## Solution 1

```python
def greet(name):
    return f"Hello, {name}"
```

---

## Solution 2

```python
def add(a, b):
    return a + b
```

---

## Solution 3

```python
def min_max(nums):
    if not nums:
        raise ValueError("nums must not be empty")

    minimum = nums[0]
    maximum = nums[0]

    for num in nums:
        if num < minimum:
            minimum = num

        if num > maximum:
            maximum = num

    return minimum, maximum
```

---

## Solution 4

```python
def average(nums):
    if not nums:
        raise ValueError("nums must not be empty")

    return sum(nums) / len(nums)
```

---

## Solution 5

```python
def create_user(name, role="user"):
    return {
        "name": name,
        "role": role
    }
```

---

## Solution 6

```python
def add_tag(tag, tags=None):
    if tags is None:
        tags = []

    tags.append(tag)

    return tags
```

---

## Solution 7

```python
def multiply_all(*numbers):
    if not numbers:
        return 1

    result = 1

    for number in numbers:
        result *= number

    return result
```

---

## Solution 8

```python
def build_config(**options):
    config = {
        "timeout": 30,
        "retries": 3
    }

    config.update(options)

    return config
```

---

## Solution 9

```python
def apply_to_list(items, func):
    result = []

    for item in items:
        result.append(func(item))

    return result
```

Example:

```python
squares = apply_to_list([1, 2, 3], lambda x: x * x)
```

---

## Solution 10

```python
def make_prefixer(prefix):
    def add_prefix(text):
        return f"{prefix}{text}"

    return add_prefix
```

Usage:

```python
add_error = make_prefixer("ERROR: ")

message = add_error("Invalid payload")
```

---

## Solution 11

```python
users = [
    {"name": "Ali", "age": 30},
    {"name": "Sara", "age": 25},
    {"name": "Omar", "age": 35}
]

users.sort(key=lambda user: user["age"])
```

---

## Solution 12

```python
def normalize_text(text):
    return " ".join(
        text.strip().lower().split()
    )
```

---

## Solution 13

```python
import hashlib

def normalize_text(text):
    return " ".join(
        text.strip().lower().split()
    )

def build_embedding_cache_key(model_name, version, text):
    normalized = normalize_text(text)

    text_hash = hashlib.sha256(
        normalized.encode("utf-8")
    ).hexdigest()

    return model_name, version, text_hash
```

---

## Solution 14

```python
def is_valid_email(email):
    if "@" not in email:
        return False

    local, domain = email.split("@", 1)

    if not local:
        return False

    if "." not in domain:
        return False

    return True
```

This is simple validation, not full RFC-compliant email validation.

---

## Solution 15

```python
def retry(func, attempts=3):
    last_error = None

    for _ in range(attempts):
        try:
            return func()
        except Exception as error:
            last_error = error

    raise RuntimeError("Function failed after retries") from last_error
```

---

# Mock Interview

## Interviewer

What is a function?

## Strong Candidate Answer

A function is a reusable block of code that accepts inputs through parameters, performs logic, and optionally returns an output. Functions help organize code, reduce duplication, and improve testability.

---

## Interviewer

What is the difference between parameter and argument?

## Strong Candidate Answer

A parameter is defined in the function signature. An argument is the actual value passed when calling the function.

---

## Interviewer

Why are mutable default arguments dangerous?

## Strong Candidate Answer

Default arguments are evaluated once at function definition time. If the default is mutable, like a list or dictionary, the same object is reused across calls. This can cause unexpected shared state.

---

## Interviewer

What is a closure?

## Strong Candidate Answer

A closure is a function that remembers variables from its enclosing scope even after that outer function has finished executing.

---

## Interviewer

How would you make an API-calling function testable?

## Strong Candidate Answer

I would inject the API client as a dependency instead of creating or using a global client inside the function. That allows tests to pass a fake client.

---

## Interviewer

How would functions appear in a RAG pipeline?

## Strong Candidate Answer

I would split the pipeline into small functions: normalize query, retrieve documents, rerank documents, build prompt, call the LLM, and post-process the response. This makes the system easier to test, debug, monitor, and replace piece by piece.

---

# Revision Sheet

## Basic Function

```python
def add(a, b):
    return a + b
```

---

## Default Argument

```python
def greet(name, title="Mr"):
    return f"Hello {title}. {name}"
```

---

## Safe Mutable Default

```python
def collect(item, items=None):
    if items is None:
        items = []

    items.append(item)

    return items
```

---

## *args

```python
def add_all(*numbers):
    return sum(numbers)
```

`numbers` is a tuple.

---

## **kwargs

```python
def create_user(**kwargs):
    return kwargs
```

`kwargs` is a dictionary.

---

## Unpack List

```python
values = [1, 2, 3]

func(*values)
```

---

## Unpack Dict

```python
data = {"name": "Abubaker"}

func(**data)
```

---

## Lambda

```python
items.sort(key=lambda x: x["score"])
```

---

## Closure

```python
def outer(value):
    def inner():
        return value

    return inner
```

---

## Type Hints

```python
def normalize_text(text: str) -> str:
    return text.strip().lower()
```

---

# Cheat Sheet

| Task                     | Code                                    |
| ------------------------ | --------------------------------------- |
| Define function          | `def func():`                           |
| Return value             | `return value`                          |
| Default arg              | `def func(x=1):`                        |
| Variable positional args | `*args`                                 |
| Variable keyword args    | `**kwargs`                              |
| Lambda                   | `lambda x: x * 2`                       |
| Type hint                | `def f(x: int) -> int:`                 |
| Local scope              | variable inside function                |
| Global scope             | module-level variable                   |
| Closure                  | inner function remembers outer variable |
| Safe default list        | use `None`                              |

---

# Final Key Takeaways

Functions are not just syntax.

They are the foundation of clean software design.

Master:

* Parameters
* Return values
* Default arguments
* `*args`
* `**kwargs`
* Scope
* Closures
* Lambdas
* Type hints
* Pure functions
* Side effects
* Testability

For interviews, always write functions that are:

* Clear
* Small
* Predictable
* Testable
* Easy to reason about

For AI engineering, good functions help you build pipelines that are modular, observable, and production-ready.
