# Python Memory Model

> Understanding how Python stores, references, copies, and destroys objects.

---

# Why This Matters

Many Python developers know syntax but do not understand what happens in memory.

Senior interviewers frequently ask questions that are actually testing:

* References
* Mutability
* Identity
* Object lifecycle
* Garbage collection
* Copying behavior

rather than Python syntax itself.

This chapter is foundational for:

* Lists
* Dictionaries
* Functions
* OOP
* Concurrency
* FastAPI
* AI Systems

---

# Learning Objectives

By the end of this chapter you should be able to:

* Explain the difference between a variable and an object
* Explain references
* Explain mutability
* Explain object identity
* Explain shallow vs deep copy
* Explain Python garbage collection
* Debug reference-related bugs
* Analyze memory-related interview questions

---

# Variables Are Not Objects

Many beginners imagine:

```python
x = 10
```

as:

```text
x contains 10
```

This is incorrect.

Python variables hold references to objects.

Instead:

```text
x
│
▼
┌────┐
│ 10 │
└────┘
```

The variable points to an object.

---

# Objects

Everything in Python is an object.

Examples:

```python
10
3.14
"hello"
[1, 2, 3]
{"name": "Abubaker"}
```

All are objects.

Each object has:

* Identity
* Type
* Value

---

# Example

```python
x = 10

print(type(x))
```

Output:

```python
<class 'int'>
```

---

# Assignment

Consider:

```python
a = [1, 2, 3]
```

Memory:

```text
a
│
▼
┌───────────┐
│ [1,2,3]   │
└───────────┘
```

Now:

```python
b = a
```

Memory:

```text
      ┌───────────┐
a ───►│ [1,2,3]   │
      └───────────┘
           ▲
           │
b ─────────┘
```

Important:

Python did NOT copy the list.

Python copied the reference.

---

# Demonstration

```python
a = [1, 2, 3]
b = a

b.append(4)

print(a)
```

Output:

```python
[1, 2, 3, 4]
```

Why?

Both variables point to the same object.

---

# Identity

Every object has an identity.

Python exposes this via:

```python
id()
```

Example:

```python
a = [1, 2, 3]

print(id(a))
```

Output:

```text
140604228817920
```

(The value varies.)

---

# Equality vs Identity

One of the most common interview questions.

```python
a = [1, 2]
b = [1, 2]
```

Value comparison:

```python
a == b
```

Result:

```python
True
```

Identity comparison:

```python
a is b
```

Result:

```python
False
```

Why?

```text
a ─► [1,2]

b ─► [1,2]
```

Different objects.

Same values.

---

# Mutable Objects

Mutable means:

The object can change after creation.

Examples:

```python
list
dict
set
bytearray
```

---

# Example

```python
data = [1, 2]

data.append(3)
```

Object changed.

---

# Immutable Objects

Immutable means:

The object cannot be modified after creation.

Examples:

```python
int
float
str
tuple
bool
frozenset
```

---

# Example

```python
x = 10

x += 1
```

Python creates a new object.

Memory:

Before:

```text
x ─► 10
```

After:

```text
x ─► 11
```

The original integer remains unchanged.

---

# Interview Trap

```python
a = 10
b = a

b += 1
```

Question:

Does changing b modify a?

Answer:

No.

Integers are immutable.

---

# Function Arguments

Python passes references to objects.

Example:

```python
def add_item(items):
    items.append("new")

data = []

add_item(data)

print(data)
```

Output:

```python
['new']
```

The function received a reference to the same list.

---

# Visualizing Function Calls

```python
data = []
```

Memory:

```text
data
│
▼
[]
```

Function call:

```python
add_item(data)
```

Inside function:

```text
data ─┐
      │
      ▼
items ─► []
```

Both names reference the same object.

---

# Mutable Default Argument Bug

Classic interview question.

Bad:

```python
def add(value, items=[]):
    items.append(value)
    return items
```

Calls:

```python
print(add(1))
print(add(2))
print(add(3))
```

Output:

```python
[1]
[1, 2]
[1, 2, 3]
```

Unexpected.

---

# Why It Happens

Default arguments are evaluated once.

The same list is reused.

Memory:

```text
Function Definition
        │
        ▼
      []
```

All calls reuse it.

---

# Correct Solution

```python
def add(value, items=None):

    if items is None:
        items = []

    items.append(value)

    return items
```

Always use this pattern.

---

# Copying Objects

There are two major types:

1. Shallow Copy
2. Deep Copy

---

# Shallow Copy

```python
import copy

a = [[1], [2]]

b = copy.copy(a)
```

Memory:

```text
a ─► [A, B]

b ─► [A, B]
```

Outer list copied.

Inner lists shared.

---

# Problem

```python
a = [[1], [2]]

b = copy.copy(a)

b[0].append(99)

print(a)
```

Output:

```python
[[1, 99], [2]]
```

Shared nested object.

---

# Deep Copy

```python
import copy

a = [[1], [2]]

b = copy.deepcopy(a)
```

Memory:

```text
a ─► [A, B]

b ─► [C, D]
```

Everything copied.

---

# Garbage Collection

Memory must eventually be released.

Python primarily uses:

1. Reference Counting
2. Cyclic Garbage Collection

---

# Reference Counting

Imagine:

```python
a = []
b = a
c = a
```

Memory:

```text
a ─┐
b ─┼──► []
c ─┘
```

Reference count:

```text
3
```

Delete:

```python
del a
del b
del c
```

Reference count becomes:

```text
0
```

Object becomes collectible.

---

# Cyclic References

Reference counting alone cannot solve:

```python
class Node:
    pass

a = Node()
b = Node()

a.next = b
b.next = a
```

Diagram:

```text
a ─► b
▲    │
│    ▼
└────┘
```

Python's garbage collector detects such cycles.

---

# Real AI Engineering Example

Suppose:

```python
embedding_cache = {}
```

Stores:

```python
document_id -> embedding
```

Bad implementation:

```python
cache = {}

def get_embedding(doc):
    if doc not in cache:
        cache[doc] = embed(doc)

    return cache[doc]
```

Problem:

Cache grows forever.

Result:

* High RAM usage
* Slow service
* OOM crashes

Senior engineers recognize this immediately.

Solutions:

* LRU Cache
* TTL Cache
* Redis
* Cache Eviction Policies

---

# Common Production Bugs

## Bug #1

Unexpected list modification.

```python
a = [1, 2]
b = a

b.append(3)
```

---

## Bug #2

Mutable default argument.

```python
def process(data=[]):
    pass
```

---

## Bug #3

Incorrect shallow copy.

```python
copy.copy()
```

instead of:

```python
copy.deepcopy()
```

---

# Exercises

## Exercise 1

Predict output:

```python
a = [1, 2]
b = a

b.append(3)

print(a)
```

---

## Exercise 2

Predict output:

```python
a = 10
b = a

b += 5

print(a)
```

---

## Exercise 3

Predict:

```python
a = [1]
b = [1]

print(a == b)
print(a is b)
```

---

## Exercise 4

Explain:

```python
x = [[1]]

y = copy.copy(x)

y[0].append(99)
```

---

## Exercise 5

Fix:

```python
def add(value, items=[]):
    items.append(value)
    return items
```

---

# Solutions

## Solution 1

Output:

```python
[1, 2, 3]
```

Reason:

Shared reference.

---

## Solution 2

Output:

```python
10
```

Reason:

Integer immutable.

---

## Solution 3

Output:

```python
True
False
```

Reason:

Value equality vs identity.

---

## Solution 4

Both lists share nested object.

Use:

```python
copy.deepcopy()
```

---

## Solution 5

```python
def add(value, items=None):

    if items is None:
        items = []

    items.append(value)

    return items
```

---

# Interview Questions

## Junior

1. What is an object?
2. What is a variable?
3. What is mutability?
4. What is identity?
5. Difference between == and is?

## Mid-Level

6. Explain shallow copy.
7. Explain deep copy.
8. Why are mutable defaults dangerous?
9. How are function arguments passed?
10. What does id() return?

## Senior

11. Explain Python memory management.
12. Explain reference counting.
13. Explain cyclic references.
14. Explain garbage collection.
15. Describe a memory leak scenario.

---

# Interview Answers

## What is the difference between == and is?

== compares values.

is compares identities.

---

## Why are mutable defaults dangerous?

Because they are evaluated once and reused across function calls.

---

## What is shallow copy?

Copies outer container.

Nested objects remain shared.

---

## What is deep copy?

Recursively copies all nested objects.

---

## How does Python manage memory?

CPython primarily uses reference counting and supplements it with cyclic garbage collection.

---

# Mock Interview

### Interviewer

Explain:

```python
a = [1, 2, 3]
b = a

b.append(4)
```

### Strong Candidate Answer

1. List object created.
2. a references list.
3. b receives same reference.
4. append modifies object in place.
5. Both variables observe change.

---

# Revision Sheet

## Remember

```text
Variable != Object

Variable -> Reference -> Object
```

```text
==  => value

is  => identity
```

```text
Mutable

list
dict
set
```

```text
Immutable

int
float
str
tuple
bool
```

```text
copy.copy()
```

Shallow.

```text
copy.deepcopy()
```

Deep.

```text
Mutable default arguments
```

Dangerous.

---

# Key Takeaways

* Variables store references
* Objects live independently of variable names
* Mutable objects can change in place
* Immutable objects create new values
* == compares values
* is compares identities
* Assignment copies references
* Shallow copy shares nested objects
* Deep copy duplicates nested objects
* Python uses reference counting and garbage collection
* Understanding memory behavior is essential for senior Python interviews
