# Python Debugging

> Tracebacks, error diagnosis, logical bugs, mutable-state bugs, production debugging, and AI engineering troubleshooting.

---

# Why Debugging Matters

Debugging is one of the most important real-world engineering skills.

Writing code is only part of the job.

Senior engineers are expected to:

* Understand errors quickly
* Read tracebacks confidently
* Reproduce bugs
* Isolate root causes
* Fix issues without breaking other behavior
* Explain what happened clearly
* Prevent similar bugs from happening again

In interviews, debugging questions test whether you understand Python deeply.

In production, debugging skill is often more valuable than memorizing algorithms.

For AI Engineer and Backend Engineer roles, debugging appears in:

* FastAPI services
* data pipelines
* ML preprocessing
* RAG pipelines
* vector search
* API integrations
* async tasks
* Kubernetes services
* model inference systems

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Read Python tracebacks
* Identify common Python exceptions
* Debug logical errors
* Debug mutable-object issues
* Debug dictionary and list errors
* Debug function behavior
* Use print debugging effectively
* Use logging instead of print in production
* Use `pdb` basics
* Create minimal reproducible examples
* Add assertions
* Write small tests to verify fixes
* Debug AI pipeline issues step by step

---

# Debugging Mindset

A bug is not random.

A bug usually means one of these is wrong:

```text
Input
Assumption
State
Type
Control flow
Data structure choice
External dependency
```

Good debugging is systematic.

Bad debugging looks like:

```text
Change random things until it works.
```

Good debugging looks like:

```text
1. Reproduce the issue
2. Read the error
3. Identify where it happens
4. Inspect inputs and state
5. Form a hypothesis
6. Test the hypothesis
7. Fix the root cause
8. Add a test or guard
```

---

# The Debugging Loop

Use this process:

```text
Bug appears
↓
Reproduce
↓
Read traceback / observe behavior
↓
Locate failing line
↓
Inspect variables
↓
Identify wrong assumption
↓
Fix
↓
Test
↓
Prevent recurrence
```

This is how senior engineers debug.

---

# Reading Tracebacks

A traceback tells you where Python failed.

Example:

```python
def divide(a, b):
    return a / b

result = divide(10, 0)
```

Output:

```text
Traceback (most recent call last):
  File "app.py", line 4, in <module>
    result = divide(10, 0)
  File "app.py", line 2, in divide
    return a / b
ZeroDivisionError: division by zero
```

Important parts:

```text
Exception type:
ZeroDivisionError

Error message:
division by zero

Failing line:
return a / b
```

Usually start reading from the bottom.

---

# How To Read A Traceback

Given:

```text
Traceback (most recent call last):
  File "main.py", line 10, in <module>
    process_user(user)
  File "main.py", line 6, in process_user
    print(user["email"])
KeyError: 'email'
```

Ask:

1. What exception happened?
2. What line failed?
3. What value caused it?
4. What assumption was wrong?

Answer:

```text
The code expected user["email"] to exist,
but the dictionary did not contain the key "email".
```

---

# Common Exception Types

| Exception             | Meaning                           |
| --------------------- | --------------------------------- |
| `NameError`           | Variable not defined              |
| `TypeError`           | Wrong type or invalid operation   |
| `ValueError`          | Correct type, invalid value       |
| `KeyError`            | Missing dictionary key            |
| `IndexError`          | Invalid list/tuple index          |
| `AttributeError`      | Object does not have attribute    |
| `ZeroDivisionError`   | Division by zero                  |
| `FileNotFoundError`   | File path does not exist          |
| `ImportError`         | Import failed                     |
| `ModuleNotFoundError` | Module not installed or not found |

---

# NameError

Example:

```python
print(username)
```

Error:

```text
NameError: name 'username' is not defined
```

Cause:

```text
The variable was used before assignment.
```

Fix:

```python
username = "Abubaker"
print(username)
```

Common causes:

* typo in variable name
* variable defined inside another scope
* code path did not assign the variable

---

# TypeError

Example:

```python
age = 30
message = "Age: " + age
```

Error:

```text
TypeError: can only concatenate str (not "int") to str
```

Cause:

```text
Cannot concatenate string and integer directly.
```

Fix:

```python
message = "Age: " + str(age)
```

Better:

```python
message = f"Age: {age}"
```

---

# ValueError

Example:

```python
number = int("abc")
```

Error:

```text
ValueError: invalid literal for int() with base 10: 'abc'
```

Cause:

```text
The type is acceptable, but the value is invalid.
```

Fix:

```python
def parse_int(value):
    try:
        return int(value)
    except ValueError:
        return None
```

---

# KeyError

Example:

```python
user = {
    "name": "Abubaker"
}

print(user["email"])
```

Error:

```text
KeyError: 'email'
```

Cause:

```text
The dictionary does not contain the key.
```

Fix options:

```python
email = user.get("email")
```

or:

```python
email = user.get("email", "unknown@example.com")
```

or validate:

```python
if "email" not in user:
    raise ValueError("Missing email")
```

Correct choice depends on whether the key is optional or required.

---

# IndexError

Example:

```python
nums = [10, 20, 30]

print(nums[5])
```

Error:

```text
IndexError: list index out of range
```

Cause:

```text
Index 5 does not exist.
```

Fix:

```python
if index < len(nums):
    print(nums[index])
```

Better design:

```python
def safe_get(items, index, default=None):
    if 0 <= index < len(items):
        return items[index]

    return default
```

---

# AttributeError

Example:

```python
text = None

print(text.lower())
```

Error:

```text
AttributeError: 'NoneType' object has no attribute 'lower'
```

Cause:

```text
Expected string, received None.
```

Fix:

```python
if text is not None:
    normalized = text.lower()
else:
    normalized = ""
```

Or fail early:

```python
def normalize_text(text: str) -> str:
    if text is None:
        raise ValueError("text must not be None")

    return text.strip().lower()
```

---

# Debugging With print()

The simplest debugging tool is `print()`.

Example:

```python
def calculate_discount(price, discount):
    print("price:", price)
    print("discount:", discount)

    return price - discount
```

Use print debugging to inspect:

* inputs
* intermediate values
* function calls
* branch execution
* loop behavior

But avoid leaving random print statements in production code.

---

# Better Print Debugging

Bad:

```python
print("here")
print("test")
print(x)
```

Better:

```python
print(f"[calculate_discount] price={price}, discount={discount}")
```

Why better?

* includes function context
* includes variable names
* easier to search logs
* easier to understand later

---

# Debugging With Logging

In production code, use logging instead of print.

Example:

```python
import logging

logger = logging.getLogger(__name__)

def process_user(user):
    logger.info("Processing user_id=%s", user.get("id"))
```

Logging levels:

| Level    | Use                         |
| -------- | --------------------------- |
| DEBUG    | Detailed diagnostic info    |
| INFO     | Normal operational messages |
| WARNING  | Unexpected but recoverable  |
| ERROR    | Operation failed            |
| CRITICAL | Severe failure              |

Example:

```python
logger.debug("Raw payload: %s", payload)
logger.info("Processing document_id=%s", document_id)
logger.warning("Missing optional field: source")
logger.error("Failed to process document_id=%s", document_id)
```

---

# Debugging With pdb

`pdb` is Python's built-in debugger.

Example:

```python
def add(a, b):
    breakpoint()
    return a + b

add(3, 4)
```

When execution reaches `breakpoint()`, Python pauses.

Useful commands:

| Command      | Meaning            |
| ------------ | ------------------ |
| `n`          | next line          |
| `s`          | step into function |
| `c`          | continue           |
| `p variable` | print variable     |
| `l`          | list code          |
| `q`          | quit               |

Example:

```text
(Pdb) p a
3
(Pdb) p b
4
(Pdb) n
```

Use `pdb` when print debugging becomes messy.

---

# Assertions

Assertions check assumptions.

Example:

```python
def average(nums):
    assert len(nums) > 0, "nums must not be empty"

    return sum(nums) / len(nums)
```

If assumption fails:

```text
AssertionError: nums must not be empty
```

Assertions are good for development checks.

For user-facing validation, prefer explicit exceptions:

```python
def average(nums):
    if not nums:
        raise ValueError("nums must not be empty")

    return sum(nums) / len(nums)
```

---

# Creating Minimal Reproducible Examples

When debugging, reduce the problem.

Bad:

```text
The whole service is broken.
```

Better:

```python
payload = {
    "name": "Abubaker"
}

print(payload["email"])
```

This reproduces the exact `KeyError`.

A minimal example should contain:

* smallest input
* smallest code
* same failure
* no unrelated logic

This is essential for debugging production issues quickly.

---

# Debugging Logical Bugs

Not all bugs raise exceptions.

Example:

```python
def is_adult(age):
    return age > 18
```

Bug:

```text
Age 18 should maybe count as adult.
```

Correct:

```python
def is_adult(age):
    return age >= 18
```

This kind of bug requires:

* tests
* examples
* careful requirement reading

---

# Off-By-One Errors

Common in loops and indexes.

Bug:

```python
def print_items(items):
    for i in range(len(items) + 1):
        print(items[i])
```

Error:

```text
IndexError
```

Correct:

```python
def print_items(items):
    for i in range(len(items)):
        print(items[i])
```

Better:

```python
def print_items(items):
    for item in items:
        print(item)
```

Prefer direct iteration when index is not needed.

---

# Debugging Mutable State

Mutable state causes many Python bugs.

Example:

```python
a = [1, 2]
b = a

b.append(3)

print(a)
```

Output:

```python
[1, 2, 3]
```

Bug cause:

```text
a and b reference the same list.
```

Fix if copy is needed:

```python
b = a.copy()
```

For nested objects:

```python
import copy

b = copy.deepcopy(a)
```

---

# Debugging Shared Nested Lists

Classic bug:

```python
matrix = [[0] * 3] * 3

matrix[0][0] = 1

print(matrix)
```

Output:

```python
[
    [1, 0, 0],
    [1, 0, 0],
    [1, 0, 0]
]
```

Cause:

```text
All rows reference the same list.
```

Fix:

```python
matrix = [
    [0] * 3
    for _ in range(3)
]
```

---

# Debugging Mutable Default Arguments

Bug:

```python
def add_item(item, items=[]):
    items.append(item)
    return items

print(add_item("a"))
print(add_item("b"))
```

Output:

```python
['a']
['a', 'b']
```

Cause:

```text
Default list is created once and reused.
```

Fix:

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)

    return items
```

---

# Debugging None Bugs

Common production bug:

```python
def normalize_text(text):
    return text.strip().lower()
```

Input:

```python
normalize_text(None)
```

Error:

```text
AttributeError: 'NoneType' object has no attribute 'strip'
```

Fix:

```python
def normalize_text(text):
    if text is None:
        return ""

    return text.strip().lower()
```

Or stricter:

```python
def normalize_text(text):
    if text is None:
        raise ValueError("text must not be None")

    return text.strip().lower()
```

Choose based on business rules.

---

# Debugging Dictionary Bugs

## Missing Key

```python
doc = {
    "id": "doc_1",
    "text": "Python debugging"
}

source = doc["metadata"]["source"]
```

Error:

```text
KeyError: 'metadata'
```

Safer:

```python
source = doc.get("metadata", {}).get("source", "unknown")
```

But be careful.

If `metadata` is required, hiding the error may be wrong.

Better:

```python
def get_document_source(doc):
    if "metadata" not in doc:
        raise ValueError("Document missing metadata")

    return doc["metadata"].get("source", "unknown")
```

---

# Debugging While Modifying A List

Bug:

```python
nums = [1, 2, 3, 4, 5]

for num in nums:
    if num % 2 == 0:
        nums.remove(num)
```

This can skip elements because the list changes while iterating.

Better:

```python
nums = [
    num
    for num in nums
    if num % 2 != 0
]
```

Or create a new list:

```python
result = []

for num in nums:
    if num % 2 != 0:
        result.append(num)
```

---

# Debugging While Modifying A Dictionary

Bug:

```python
data = {
    "name": "Abubaker",
    "age": None,
    "country": "UAE"
}

for key in data:
    if data[key] is None:
        del data[key]
```

This may raise:

```text
RuntimeError: dictionary changed size during iteration
```

Fix:

```python
data = {
    key: value
    for key, value in data.items()
    if value is not None
}
```

---

# Debugging Function Inputs

A strong debugging technique is validating function inputs.

Example:

```python
def calculate_total(items):
    if not isinstance(items, list):
        raise TypeError("items must be a list")

    total = 0

    for item in items:
        total += item["price"]

    return total
```

This fails early with a clear message.

Better than mysterious errors later.

---

# Debugging With Type Hints

Type hints help prevent bugs before runtime.

Example:

```python
def normalize_text(text: str) -> str:
    return text.strip().lower()
```

This tells readers and tools:

```text
text should be a string
return value is a string
```

Type hints do not enforce runtime behavior by themselves, but they improve readability and support static analysis tools.

---

# Debugging With Tests

Tests are executable debugging.

Example:

```python
def normalize_text(text):
    return " ".join(text.strip().lower().split())
```

Test:

```python
def test_normalize_text():
    assert normalize_text("  Hello   WORLD ") == "hello world"
```

A test protects the fix from breaking later.

---

# Debugging Strategy For Coding Interviews

When code fails in an interview:

1. Stay calm
2. Read the error
3. Use a small example
4. Walk through variables manually
5. Check edge cases
6. Fix incrementally
7. Re-run mentally or with tests

Say your reasoning out loud:

```text
I see the error is IndexError, so I am probably accessing past the end of the list. Let me check the loop boundary.
```

This is better than silently panicking.

---

# Common Interview Debugging Cases

## Case 1: Empty Input

Bug:

```python
def find_max(nums):
    maximum = nums[0]

    for num in nums:
        if num > maximum:
            maximum = num

    return maximum
```

Input:

```python
[]
```

Error:

```text
IndexError
```

Fix:

```python
def find_max(nums):
    if not nums:
        raise ValueError("nums must not be empty")

    maximum = nums[0]

    for num in nums:
        if num > maximum:
            maximum = num

    return maximum
```

---

## Case 2: Wrong Initial Value

Bug:

```python
def max_positive(nums):
    best = 0

    for num in nums:
        if num > best:
            best = num

    return best
```

Input:

```python
[-5, -2, -9]
```

Output:

```text
0
```

But `0` is not in the list.

Fix:

```python
def max_value(nums):
    if not nums:
        raise ValueError("nums must not be empty")

    best = nums[0]

    for num in nums:
        if num > best:
            best = num

    return best
```

---

## Case 3: Incorrect Loop Boundary

Bug:

```python
def pair_sum(nums):
    pairs = []

    for i in range(len(nums)):
        pairs.append(nums[i] + nums[i + 1])

    return pairs
```

Error:

```text
IndexError
```

Fix:

```python
def pair_sum(nums):
    pairs = []

    for i in range(len(nums) - 1):
        pairs.append(nums[i] + nums[i + 1])

    return pairs
```

---

# Production Debugging

Production debugging is different from local debugging.

In production, you may not have direct access to:

* local variables
* debugger
* full input payloads
* user data
* exact environment

You rely on:

* logs
* metrics
* traces
* error reports
* dashboards
* reproduction steps

---

# What To Log

Good logs include:

* request ID
* user ID or tenant ID if safe
* document ID
* operation name
* status
* duration
* error message
* important metadata

Example:

```python
logger.info(
    "Embedding request completed document_id=%s duration_ms=%s model=%s",
    document_id,
    duration_ms,
    model_name
)
```

Avoid logging:

* passwords
* API keys
* tokens
* full personal data
* sensitive documents
* private prompts

---

# Debugging AI Pipelines

AI pipelines are harder to debug because outputs can be non-deterministic.

Common failure points:

```text
Bad input text
Bad chunking
Duplicate chunks
Wrong metadata
Embedding model mismatch
Vector DB filter issue
Low-quality retrieval
Bad prompt
LLM hallucination
Timeouts
Rate limits
```

Debug step by step.

---

# RAG Debugging Checklist

When a RAG answer is bad, ask:

## Input

* Was the user query correct?
* Was the query normalized?
* Was the query rewritten incorrectly?

## Retrieval

* Were relevant documents retrieved?
* What were the top-k scores?
* Were documents filtered out incorrectly?
* Were duplicate chunks returned?

## Context

* Was the context too long?
* Was important context missing?
* Was irrelevant context included?

## Prompt

* Was the prompt clear?
* Did the prompt include instructions?
* Did the prompt ask for citations?

## Model

* Was the model appropriate?
* Was temperature too high?
* Did the model ignore context?

---

# AI Debugging Example: Duplicate Chunks

Bug:

```python
chunks = [
    "Python functions are reusable.",
    " python   functions are reusable. ",
    "Dictionaries map keys to values."
]
```

Naive deduplication:

```python
unique = list(set(chunks))
```

Problem:

```text
The first two chunks are semantically same but text differs in spaces/case.
```

Better:

```python
def normalize_text(text):
    return " ".join(text.strip().lower().split())

seen = set()
unique = []

for chunk in chunks:
    key = normalize_text(chunk)

    if key in seen:
        continue

    seen.add(key)
    unique.append(chunk)
```

---

# AI Debugging Example: Wrong Embedding Cache

Bug:

```python
cache[text] = embedding
```

Problem:

Different embedding models may use the same text.

This can return embeddings from the wrong model.

Better:

```python
key = (model_name, embedding_version, text_hash)
cache[key] = embedding
```

---

# AI Debugging Example: Metadata Missing

Bug:

```python
source = document["metadata"]["source"]
```

Some documents do not have metadata.

Fix:

```python
metadata = document.get("metadata") or {}
source = metadata.get("source", "unknown")
```

Or strict validation:

```python
if "metadata" not in document:
    raise ValueError(f"Document missing metadata: {document.get('id')}")
```

The right choice depends on whether metadata is required.

---

# Debugging Performance Issues

If code is slow, do not guess.

Ask:

1. Is it CPU-bound?
2. Is it I/O-bound?
3. Is it waiting for network?
4. Is it doing repeated work?
5. Is it copying large data?
6. Is it using the wrong data structure?
7. Is it calling an external API inside a loop?

Example slow code:

```python
for doc in documents:
    embedding = embed(doc)
```

Maybe the problem is not Python.

Maybe it is:

```text
API latency * number of documents
```

Possible fixes:

* batching
* caching
* async calls
* concurrency
* rate limiting
* background jobs

---

# Common Debugging Tools

| Tool           | Use                    |
| -------------- | ---------------------- |
| `print()`      | Quick local inspection |
| `logging`      | Production diagnostics |
| `breakpoint()` | Interactive debugging  |
| `pdb`          | Step-through debugging |
| `assert`       | Check assumptions      |
| `pytest`       | Regression tests       |
| `timeit`       | Micro-benchmarking     |
| `cProfile`     | Profiling runtime      |
| `tracemalloc`  | Memory debugging       |

---

# Interview Questions And Answers

## Q1. How do you debug a Python error?

I start by reading the traceback from the bottom to identify the exception type, message, and failing line. Then I inspect the inputs and variables around that line, reproduce the bug with a small example, fix the root cause, and add a test if appropriate.

---

## Q2. What is a traceback?

A traceback is Python's error report showing the call stack that led to an exception. It includes file names, line numbers, function calls, the exception type, and the error message.

---

## Q3. What is the difference between `KeyError` and `IndexError`?

`KeyError` happens when a dictionary key is missing.

`IndexError` happens when a list or tuple index is out of range.

---

## Q4. Why is modifying a list while iterating dangerous?

Because changing the list shifts elements while the loop is still moving through it. This can skip elements or produce unexpected behavior.

A safer approach is to create a new list or iterate over a copy.

---

## Q5. What causes the mutable default argument bug?

Default argument values are evaluated once when the function is defined. If the default is a list or dictionary, the same object is reused across calls.

---

## Q6. How do you debug a `NoneType` error?

I find where the variable became `None`, inspect the function or data source that produced it, and decide whether `None` is valid. Then I either handle it explicitly or raise a clear validation error.

---

## Q7. When should you use logging instead of print?

Use logging in production or shared code because logs can include levels, timestamps, modules, request IDs, and structured context. `print()` is acceptable for quick local debugging but should not be the main production debugging tool.

---

## Q8. What is a minimal reproducible example?

It is the smallest piece of code and input that reproduces the bug. It removes unrelated complexity and helps identify the root cause quickly.

---

## Q9. How would you debug a bad RAG answer?

I would inspect each stage:

* query
* retrieval results
* document scores
* metadata filters
* context sent to the LLM
* prompt
* model output

I would verify whether the right documents were retrieved before blaming the LLM.

---

## Q10. How do you prevent the same bug from returning?

Add a test, add validation, improve logging, document the assumption, and fix the root cause rather than only patching the symptom.

---

# Senior-Level Questions And Answers

## Senior Q1. A FastAPI service sometimes returns 500. How do you debug it?

I would start with logs and tracebacks for the failing request. I would look for request ID, endpoint, payload shape, exception type, and failing line. Then I would reproduce locally with the same input if possible. If it involves dependencies, I would check database/API failures, timeouts, and environment differences. Finally, I would add validation, better error handling, and tests.

---

## Senior Q2. A service memory usage keeps growing. What do you check?

I would check:

* unbounded dictionaries or caches
* global lists accumulating data
* background tasks retaining references
* large objects not released
* circular references
* open file/network resources
* batch jobs storing everything in memory

Tools may include:

```python
tracemalloc
gc
memory_profiler
```

I would also inspect metrics over time.

---

## Senior Q3. RAG retrieval returns duplicate documents. How do you debug?

I would check:

* whether document IDs are stable
* whether chunks are duplicated during ingestion
* whether multiple retrievers return same document
* whether deduplication uses raw text instead of normalized keys
* whether metadata IDs are missing or inconsistent

Then I would add a seen set based on stable document/chunk IDs.

---

## Senior Q4. An embedding cache returns wrong results. What might be wrong?

The cache key may be incomplete.

If the key only uses text, it may mix embeddings from different models or versions.

A better key includes:

```text
model_name
embedding_version
normalized_text_hash
tenant/user scope if needed
```

---

## Senior Q5. How do you debug intermittent bugs?

Intermittent bugs often involve:

* timing
* concurrency
* external services
* mutable shared state
* unordered data
* missing retries
* race conditions

I would add structured logging, correlation IDs, metrics, and narrow down the conditions where the bug appears.

---

## Senior Q6. How do you debug code you did not write?

I first reproduce the issue and read the error. Then I identify the entry point, inputs, outputs, and dependencies. I avoid large refactors initially. I add small observations, tests, and logs until the behavior is clear. Then I make the smallest safe fix.

---

# Exercises

## Exercise 1 — Fix KeyError

Bug:

```python
def get_email(user):
    return user["email"]
```

Input:

```python
{"name": "Abubaker"}
```

Fix safely.

---

## Exercise 2 — Fix IndexError

Bug:

```python
def get_last(items):
    return items[len(items)]
```

---

## Exercise 3 — Fix Mutable Default

Bug:

```python
def add_tag(tag, tags=[]):
    tags.append(tag)
    return tags
```

---

## Exercise 4 — Fix Shared Matrix

Bug:

```python
matrix = [[0] * 3] * 3
matrix[0][0] = 1
```

Create independent rows.

---

## Exercise 5 — Fix NoneType Error

Bug:

```python
def normalize(text):
    return text.strip().lower()
```

Handle `None`.

---

## Exercise 6 — Fix List Modification During Iteration

Bug:

```python
def remove_even(nums):
    for num in nums:
        if num % 2 == 0:
            nums.remove(num)

    return nums
```

---

## Exercise 7 — Fix Dictionary Modification During Iteration

Bug:

```python
def remove_none(data):
    for key in data:
        if data[key] is None:
            del data[key]

    return data
```

---

## Exercise 8 — Add Validation

Improve:

```python
def average(nums):
    return sum(nums) / len(nums)
```

---

## Exercise 9 — Add Useful Logging

Add logging to:

```python
def process_document(document):
    return document["text"].strip()
```

---

## Exercise 10 — Debug RAG Duplicate Chunks

Given:

```python
chunks = [
    "Python Functions",
    " python   functions ",
    "Dictionaries"
]
```

Deduplicate correctly.

---

## Exercise 11 — Fix Wrong Cache Key

Bug:

```python
cache[text] = embedding
```

Build a safer embedding cache key.

---

## Exercise 12 — Minimal Reproduction

Create a minimal example for:

```text
KeyError: 'metadata'
```

---

# Solutions

## Solution 1

```python
def get_email(user):
    return user.get("email")
```

Or strict:

```python
def get_email(user):
    if "email" not in user:
        raise ValueError("Missing required field: email")

    return user["email"]
```

---

## Solution 2

```python
def get_last(items):
    if not items:
        raise ValueError("items must not be empty")

    return items[-1]
```

---

## Solution 3

```python
def add_tag(tag, tags=None):
    if tags is None:
        tags = []

    tags.append(tag)

    return tags
```

---

## Solution 4

```python
matrix = [
    [0] * 3
    for _ in range(3)
]

matrix[0][0] = 1
```

---

## Solution 5

Permissive version:

```python
def normalize(text):
    if text is None:
        return ""

    return text.strip().lower()
```

Strict version:

```python
def normalize(text):
    if text is None:
        raise ValueError("text must not be None")

    return text.strip().lower()
```

---

## Solution 6

```python
def remove_even(nums):
    return [
        num
        for num in nums
        if num % 2 != 0
    ]
```

---

## Solution 7

```python
def remove_none(data):
    return {
        key: value
        for key, value in data.items()
        if value is not None
    }
```

---

## Solution 8

```python
def average(nums):
    if not nums:
        raise ValueError("nums must not be empty")

    return sum(nums) / len(nums)
```

---

## Solution 9

```python
import logging

logger = logging.getLogger(__name__)

def process_document(document):
    document_id = document.get("id", "unknown")

    logger.info("Processing document_id=%s", document_id)

    if "text" not in document:
        logger.error("Missing text for document_id=%s", document_id)
        raise ValueError("Document missing text")

    return document["text"].strip()
```

---

## Solution 10

```python
def normalize_text(text):
    return " ".join(text.strip().lower().split())

def deduplicate_chunks(chunks):
    seen = set()
    unique = []

    for chunk in chunks:
        key = normalize_text(chunk)

        if key in seen:
            continue

        seen.add(key)
        unique.append(chunk)

    return unique
```

---

## Solution 11

```python
import hashlib

def build_embedding_cache_key(model_name, embedding_version, text):
    normalized = " ".join(text.strip().lower().split())

    text_hash = hashlib.sha256(
        normalized.encode("utf-8")
    ).hexdigest()

    return model_name, embedding_version, text_hash
```

---

## Solution 12

```python
document = {
    "id": "doc_1",
    "text": "Python debugging"
}

print(document["metadata"])
```

This reproduces:

```text
KeyError: 'metadata'
```

---

# Mock Interview

## Interviewer

Your Python code throws a traceback. What do you do first?

## Strong Candidate Answer

I read the traceback from the bottom to identify the exception type, error message, and failing line. Then I inspect the variables around that line and reproduce the issue with a small input.

---

## Interviewer

What is a common cause of `AttributeError: 'NoneType' object has no attribute ...`?

## Strong Candidate Answer

The code expected an object like a string or dictionary, but received `None`. I would trace where that value came from and decide whether to handle `None` or raise a validation error earlier.

---

## Interviewer

Why is this bug happening?

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

## Strong Candidate Answer

The default list is created once when the function is defined and reused across calls. The fix is to use `None` as the default and create a new list inside the function.

---

## Interviewer

How would you debug a RAG answer that is wrong?

## Strong Candidate Answer

I would not immediately blame the LLM. I would inspect the query, retrieved documents, scores, metadata filters, prompt, and final context. If the right documents were not retrieved, the issue is retrieval. If they were retrieved but the answer is wrong, then I would inspect prompt and generation behavior.

---

## Interviewer

How do you debug production issues when you cannot use a debugger?

## Strong Candidate Answer

I rely on structured logs, metrics, traces, request IDs, error reports, and reproducible inputs. I add targeted logging if needed and try to reproduce the issue locally or in staging.

---

# Revision Sheet

## Debugging Process

```text
Reproduce
Read error
Find failing line
Inspect variables
Form hypothesis
Test hypothesis
Fix root cause
Add test/log/validation
```

---

## Common Exceptions

| Error               | Meaning              |
| ------------------- | -------------------- |
| `NameError`         | variable not defined |
| `TypeError`         | wrong type           |
| `ValueError`        | invalid value        |
| `KeyError`          | missing dict key     |
| `IndexError`        | index out of range   |
| `AttributeError`    | missing attribute    |
| `ZeroDivisionError` | division by zero     |

---

## Safe Dictionary Access

```python
value = data.get("key", default)
```

---

## Safe List Access

```python
if 0 <= index < len(items):
    value = items[index]
```

---

## Safe Mutable Default

```python
def func(items=None):
    if items is None:
        items = []
```

---

## Avoid Modifying While Iterating

Use comprehension:

```python
items = [
    item
    for item in items
    if condition(item)
]
```

---

## Debug Tools

| Tool           | Use                     |
| -------------- | ----------------------- |
| `print()`      | quick local check       |
| `logging`      | production diagnostics  |
| `breakpoint()` | interactive debugging   |
| `assert`       | development assumptions |
| `pytest`       | regression tests        |
| `timeit`       | small benchmarks        |
| `cProfile`     | runtime profiling       |
| `tracemalloc`  | memory debugging        |

---

# Cheat Sheet

## Print Variable

```python
print(f"value={value}")
```

## Breakpoint

```python
breakpoint()
```

## Assert

```python
assert condition, "message"
```

## Raise Clear Error

```python
raise ValueError("Invalid input")
```

## Logging

```python
logger.info("Processing id=%s", item_id)
```

## Safe Get

```python
data.get("key", default)
```

## Normalize Text

```python
" ".join(text.strip().lower().split())
```

---

# Final Key Takeaways

Debugging is a senior engineering skill.

Strong engineers do not randomly change code.

They:

* reproduce the issue
* read the error carefully
* inspect assumptions
* isolate the root cause
* fix the smallest correct thing
* add prevention

For AI engineering, debugging must happen across the full pipeline:

```text
Input
↓
Preprocessing
↓
Retrieval
↓
Prompt
↓
Model
↓
Output
```

Do not guess.

Inspect each stage.

That is how production-grade engineers debug.
