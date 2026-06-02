# Core Python Cheat Sheet

> Fast revision sheet for Core Python interviews: memory model, lists, tuples, sets, dictionaries, functions, complexity, debugging, performance, and AI engineering patterns.

---

# How To Use This Cheat Sheet

Use this file for quick revision before interviews.

This is not a replacement for the full chapters.

Use it to quickly recall:

* definitions
* common patterns
* complexity
* interview traps
* production notes
* AI engineering examples

Recommended usage:

```text
1. Review once daily during interview prep.
2. Rewrite key parts from memory.
3. Practice explaining each section out loud.
4. Use this before mock interviews.
```

---

# 1. Python Memory Model

## Variables Reference Objects

```python
x = [1, 2, 3]
```

Conceptually:

```text
x ───► [1, 2, 3]
```

A variable is a name pointing to an object.

Assignment copies references, not objects.

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

---

## Equality vs Identity

```python
a == b
```

Checks value equality.

```python
a is b
```

Checks object identity.

Example:

```python
a = [1, 2]
b = [1, 2]

print(a == b)  # True
print(a is b)  # False
```

---

## Mutable Types

Mutable objects can change in place.

```text
list
dict
set
bytearray
```

Example:

```python
items = [1, 2]
items.append(3)
```

---

## Immutable Types

Immutable objects cannot be changed in place.

```text
int
float
str
tuple
bool
frozenset
```

Example:

```python
x = 10
x += 1
```

This creates/references a new integer object.

---

## Shallow Copy

Copies the outer container only.

```python
import copy

b = copy.copy(a)
```

Nested objects are still shared.

---

## Deep Copy

Copies nested objects recursively.

```python
import copy

b = copy.deepcopy(a)
```

Use when nested mutable objects must be independent.

---

## Mutable Default Argument Trap

Bad:

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

Good:

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

---

# 2. Lists

## List Basics

Lists are:

```text
ordered
mutable
dynamic
indexable
reference-based
```

```python
nums = [1, 2, 3]
```

---

## Common Operations

```python
nums.append(4)
nums.extend([5, 6])
nums.insert(0, 100)
nums.remove(2)
nums.pop()
nums.pop(0)
nums.sort()
sorted_nums = sorted(nums)
```

---

## List Complexity

| Operation       | Complexity     |
| --------------- | -------------- |
| Access by index | O(1)           |
| Update by index | O(1)           |
| Append          | O(1) amortized |
| Pop end         | O(1)           |
| Insert front    | O(n)           |
| Pop front       | O(n)           |
| Search          | O(n)           |
| Membership      | O(n)           |
| Copy            | O(n)           |
| Sort            | O(n log n)     |

---

## Append vs Insert

```python
nums.append(x)
```

Usually O(1) amortized.

```python
nums.insert(0, x)
```

O(n), because elements shift.

---

## Slicing

```python
nums[start:end:step]
```

Examples:

```python
nums[:3]     # first 3
nums[-3:]    # last 3
nums[::-1]   # reversed copy
nums[:]      # shallow copy
```

Important:

```text
Slicing creates a new list.
Time:  O(n)
Space: O(n)
```

---

## List Comprehension

```python
squares = [x * x for x in nums]
```

With filter:

```python
evens = [x for x in nums if x % 2 == 0]
```

Flatten one level:

```python
flat = [
    item
    for row in matrix
    for item in row
]
```

---

## Common List Traps

### Shared Nested Lists

Bad:

```python
matrix = [[0] * 3] * 3
```

Good:

```python
matrix = [
    [0] * 3
    for _ in range(3)
]
```

---

### Repeated Concatenation

Bad:

```python
result = []

for item in items:
    result = result + [item]
```

Good:

```python
result = []

for item in items:
    result.append(item)
```

---

# 3. Tuples

## Tuple Basics

Tuples are:

```text
ordered
immutable
indexable
fixed-size
```

```python
point = (10, 20)
```

---

## Single Element Tuple

Wrong:

```python
value = (10)
```

This is an integer.

Correct:

```python
value = (10,)
```

The comma creates the tuple.

---

## Tuple Unpacking

```python
point = (10, 20)

x, y = point
```

Swap variables:

```python
a, b = b, a
```

Extended unpacking:

```python
first, *middle, last = values
```

---

## Tuple As Dictionary Key

```python
cache_key = ("user_123", "2026-06-02")

cache[cache_key] = result
```

Tuples are hashable only if all elements are hashable.

Valid:

```python
("a", "b", 1)
```

Invalid:

```python
("a", ["b"])
```

because the tuple contains a list.

---

## Tuple vs List

| List                          | Tuple                         |
| ----------------------------- | ----------------------------- |
| Mutable                       | Immutable                     |
| Dynamic                       | Fixed                         |
| Not hashable                  | Hashable if contents hashable |
| Good for changing collections | Good for fixed records        |

---

## AI Tuple Examples

```text
(document_id, score)
(label, probability)
(model_name, text_hash)
(user_id, item_id)
(feature_name, importance)
```

---

# 4. Sets

## Set Basics

Sets are:

```text
unordered
mutable
unique values only
hash-based
fast for membership
```

```python
items = {1, 2, 3}
```

Empty set:

```python
items = set()
```

Not:

```python
items = {}
```

That creates a dictionary.

---

## Set Operations

```python
a | b   # union
a & b   # intersection
a - b   # difference
a ^ b   # symmetric difference
```

---

## Set Methods

```python
items.add(x)
items.remove(x)
items.discard(x)
items.pop()
items.clear()
```

Difference:

```python
remove()
```

raises error if missing.

```python
discard()
```

does not raise error if missing.

---

## Set Complexity

| Operation    | Complexity             |
| ------------ | ---------------------- |
| Add          | O(1) average           |
| Remove       | O(1) average           |
| Membership   | O(1) average           |
| Iteration    | O(n)                   |
| Union        | O(len(a) + len(b))     |
| Intersection | O(min(len(a), len(b))) |
| Difference   | O(len(a))              |

---

## Deduplicate

If order does not matter:

```python
unique = list(set(items))
```

If order matters:

```python
seen = set()
result = []

for item in items:
    if item not in seen:
        seen.add(item)
        result.append(item)
```

---

## Seen Set Pattern

```python
def has_duplicates(items):
    seen = set()

    for item in items:
        if item in seen:
            return True

        seen.add(item)

    return False
```

---

## frozenset

Immutable set:

```python
permissions = frozenset(["read", "write"])
```

Can be used as a dictionary key.

---

# 5. Dictionaries

## Dictionary Basics

A dictionary maps keys to values.

```python
user = {
    "name": "Abubaker",
    "role": "Software Engineer"
}
```

```text
key -> value
```

---

## Common Operations

```python
user["name"]
user.get("name")
user.get("country", "Unknown")

user["country"] = "UAE"

del user["role"]
user.pop("role")

"name" in user
```

---

## Dictionary Methods

```python
user.keys()
user.values()
user.items()
```

Iterate:

```python
for key, value in user.items():
    print(key, value)
```

---

## Dictionary Complexity

| Operation         | Complexity   |
| ----------------- | ------------ |
| Lookup by key     | O(1) average |
| Insert            | O(1) average |
| Update            | O(1) average |
| Delete            | O(1) average |
| Membership by key | O(1) average |
| Iterate           | O(n)         |
| Copy              | O(n)         |

---

## Key Requirements

Dictionary keys must be hashable.

Valid:

```python
{
    "name": "Abubaker",
    1: "one",
    (10, 20): "point"
}
```

Invalid:

```python
{
    ["a", "b"]: "invalid"
}
```

Lists are unhashable.

---

## Safe Access

Required key:

```python
value = data["key"]
```

Optional key:

```python
value = data.get("key", default)
```

---

## Frequency Counter

```python
freq = {}

for item in items:
    freq[item] = freq.get(item, 0) + 1
```

With Counter:

```python
from collections import Counter

freq = Counter(items)
```

---

## Grouping

```python
groups = {}

for item in items:
    key = get_key(item)

    if key not in groups:
        groups[key] = []

    groups[key].append(item)
```

With defaultdict:

```python
from collections import defaultdict

groups = defaultdict(list)

for item in items:
    groups[get_key(item)].append(item)
```

---

## Dictionary Comprehension

```python
squares = {
    x: x * x
    for x in nums
}
```

Filter:

```python
cleaned = {
    key: value
    for key, value in data.items()
    if value is not None
}
```

---

## Merge Dictionaries

```python
merged = a | b
```

Alternative:

```python
merged = {**a, **b}
```

---

# 6. Functions

## Basic Function

```python
def add(a, b):
    return a + b
```

No explicit return:

```python
def log():
    print("hello")
```

returns:

```python
None
```

---

## Parameters vs Arguments

Parameter:

```python
def greet(name):
    ...
```

Argument:

```python
greet("Abubaker")
```

---

## Default Arguments

```python
def greet(name, title="Mr"):
    return f"Hello {title}. {name}"
```

Avoid mutable defaults.

---

## *args

Collects extra positional arguments into a tuple.

```python
def add_all(*numbers):
    return sum(numbers)
```

---

## **kwargs

Collects extra keyword arguments into a dictionary.

```python
def create_profile(**kwargs):
    return kwargs
```

---

## Unpacking Arguments

List/tuple unpacking:

```python
values = [1, 2, 3]

func(*values)
```

Dictionary unpacking:

```python
data = {
    "name": "Abubaker",
    "role": "Engineer"
}

create_user(**data)
```

---

## Scope

Python follows LEGB:

```text
Local
Enclosing
Global
Built-in
```

---

## Closure

A function that remembers variables from an enclosing scope.

```python
def make_multiplier(factor):
    def multiply(value):
        return value * factor

    return multiply
```

---

## Lambda

```python
square = lambda x: x * x
```

Common use:

```python
users.sort(key=lambda user: user["age"])
```

Use lambda for simple one-line logic only.

---

## Pure Function

Same input -> same output.

No side effects.

```python
def add(a, b):
    return a + b
```

Pure functions are easier to test, debug, cache, and parallelize.

---

## Type Hints

```python
def normalize_text(text: str) -> str:
    return text.strip().lower()
```

---

# 7. Complexity Analysis

## Common Big O

| Complexity | Meaning      | Example           |
| ---------- | ------------ | ----------------- |
| O(1)       | Constant     | List index access |
| O(log n)   | Logarithmic  | Binary search     |
| O(n)       | Linear       | Loop over list    |
| O(n log n) | Sorting-like | sorted()          |
| O(n²)      | Quadratic    | Nested loops      |
| O(2ⁿ)      | Exponential  | Naive recursion   |

---

## Rules

Sequential loops:

```text
O(n + n) = O(n)
```

Nested loops:

```text
O(n * n) = O(n²)
```

Different inputs:

```text
O(n * m)
```

Drop constants:

```text
O(2n) = O(n)
```

Drop lower-order terms:

```text
O(n² + n) = O(n²)
```

---

## Common Data Structure Costs

| Operation         | Complexity     |
| ----------------- | -------------- |
| List membership   | O(n)           |
| Set membership    | O(1) average   |
| Dict lookup       | O(1) average   |
| List append       | O(1) amortized |
| List insert front | O(n)           |
| List sort         | O(n log n)     |

---

## Time-Space Tradeoff

Example: Two Sum.

Brute force:

```text
Time:  O(n²)
Space: O(1)
```

Dictionary solution:

```text
Time:  O(n)
Space: O(n)
```

Use extra memory to reduce runtime.

---

# 8. Common Coding Patterns

## Frequency Count

```python
freq = {}

for item in items:
    freq[item] = freq.get(item, 0) + 1
```

Use for:

```text
counting
anagrams
most frequent
histograms
```

---

## Seen Set

```python
seen = set()

for item in items:
    if item in seen:
        ...
    seen.add(item)
```

Use for:

```text
duplicates
deduplication
visited tracking
```

---

## Hash Map Lookup

```python
lookup = {}

for item in items:
    lookup[item["id"]] = item
```

Use for:

```text
fast lookup
Two Sum
ID indexing
caching
```

---

## Grouping

```python
groups = {}

for item in items:
    key = get_key(item)
    groups.setdefault(key, []).append(item)
```

---

## Two Pointers

```python
left = 0
right = len(nums) - 1

while left < right:
    ...
    left += 1
    right -= 1
```

Use for:

```text
sorted arrays
palindrome
reverse
pair search
```

---

## Sliding Window

```python
window_sum = sum(nums[:k])
best = window_sum

for right in range(k, len(nums)):
    window_sum += nums[right]
    window_sum -= nums[right - k]
    best = max(best, window_sum)
```

Use for:

```text
contiguous subarray
substring
max/min range
longest/shortest window
```

---

## Prefix Sum

```python
prefix = [0]

for num in nums:
    prefix.append(prefix[-1] + num)
```

Range sum:

```python
prefix[right + 1] - prefix[left]
```

---

## Stack

```python
stack = []

stack.append(x)
stack.pop()
```

Use for:

```text
parentheses
nested structures
undo
parsing
```

---

## Queue

```python
from collections import deque

queue = deque()
queue.append(x)
queue.popleft()
```

Use for:

```text
BFS
task queues
FIFO processing
```

---

## Heap

```python
import heapq

heapq.heappush(heap, x)
heapq.heappop(heap)
```

Top K:

```python
heapq.nlargest(k, items, key=lambda x: x["score"])
```

---

# 9. Debugging

## Debugging Process

```text
Reproduce
↓
Read traceback
↓
Find failing line
↓
Inspect variables
↓
Find wrong assumption
↓
Fix root cause
↓
Add test/log/validation
```

---

## Common Exceptions

| Exception         | Meaning              |
| ----------------- | -------------------- |
| NameError         | variable not defined |
| TypeError         | wrong type           |
| ValueError        | invalid value        |
| KeyError          | missing dict key     |
| IndexError        | index out of range   |
| AttributeError    | missing attribute    |
| ZeroDivisionError | division by zero     |
| FileNotFoundError | missing file         |

---

## Safe Dictionary Access

```python
value = data.get("key", default)
```

Nested safe access:

```python
source = doc.get("metadata", {}).get("source", "unknown")
```

Use strict validation if the field is required.

---

## Safe List Access

```python
if 0 <= index < len(items):
    value = items[index]
```

---

## Avoid Modifying List While Iterating

Bad:

```python
for item in items:
    if should_remove(item):
        items.remove(item)
```

Good:

```python
items = [
    item
    for item in items
    if not should_remove(item)
]
```

---

## Avoid Modifying Dict While Iterating

Bad:

```python
for key in data:
    if data[key] is None:
        del data[key]
```

Good:

```python
data = {
    key: value
    for key, value in data.items()
    if value is not None
}
```

---

## Logging

```python
import logging

logger = logging.getLogger(__name__)

logger.info("Processing document_id=%s", document_id)
logger.error("Failed document_id=%s", document_id)
```

Do not log:

```text
passwords
tokens
API keys
private documents
large embeddings
sensitive prompts
```

---

## breakpoint()

```python
breakpoint()
```

Useful commands:

```text
n  -> next
s  -> step into
c  -> continue
p  -> print variable
q  -> quit
```

---

# 10. Performance

## Performance Process

```text
Make it correct
Make it clear
Measure
Find bottleneck
Optimize bottleneck
Measure again
```

---

## CPU-Bound vs I/O-Bound

CPU-bound:

```text
heavy computation
image processing
large pure Python loops
numeric processing
```

Possible solutions:

```text
better algorithm
NumPy
multiprocessing
compiled libraries
```

I/O-bound:

```text
API calls
database queries
network
file I/O
external AI services
```

Possible solutions:

```text
async
threading
batching
caching
connection pooling
timeouts
retries
```

---

## Common Performance Fixes

| Problem                 | Fix            |
| ----------------------- | -------------- |
| Repeated list lookup    | Convert to set |
| Repeated list concat    | Use append     |
| Huge intermediate list  | Use generator  |
| One API call per item   | Batch calls    |
| Repeated expensive work | Cache          |
| Queue with pop(0)       | Use deque      |
| Heavy numeric loop      | Use NumPy      |
| Top-k from large list   | Use heap       |
| Large file loaded fully | Stream lines   |

---

## Timing

```python
import time

start = time.perf_counter()

run_pipeline()

end = time.perf_counter()

print(end - start)
```

---

## timeit

```python
import timeit

duration = timeit.timeit(
    stmt="target in items",
    setup="items=set(range(1000)); target=999",
    number=1000
)
```

---

## cProfile

```python
import cProfile

cProfile.run("main()")
```

---

## tracemalloc

```python
import tracemalloc

tracemalloc.start()

run_pipeline()

current, peak = tracemalloc.get_traced_memory()

tracemalloc.stop()
```

---

## Generators

```python
def normalize_texts(texts):
    for text in texts:
        yield text.strip().lower()
```

Use when data is large and one-pass processing is enough.

---

## Batching

```python
def batch_items(items, batch_size):
    if batch_size <= 0:
        raise ValueError("batch_size must be positive")

    for start in range(0, len(items), batch_size):
        yield items[start:start + batch_size]
```

---

## Caching

```python
cache = {}

if key not in cache:
    cache[key] = expensive_call()

return cache[key]
```

Controlled cache:

```python
from functools import lru_cache

@lru_cache(maxsize=1000)
def expensive_func(x):
    return x * x
```

---

# 11. AI Engineering Python Patterns

## Normalize Text

```python
def normalize_text(text: str) -> str:
    return " ".join(
        text.strip().lower().split()
    )
```

---

## Deduplicate Chunks

```python
def deduplicate_chunks(chunks: list[str]) -> list[str]:
    seen = set()
    result = []

    for chunk in chunks:
        key = normalize_text(chunk)

        if not key:
            continue

        if key in seen:
            continue

        seen.add(key)
        result.append(key)

    return result
```

---

## Build Embedding Cache Key

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

---

## Deduplicate Retrieval Results

```python
def deduplicate_results(results: list[dict]) -> list[dict]:
    seen = set()
    final = []

    for result in results:
        doc_id = result["document_id"]

        if doc_id in seen:
            continue

        seen.add(doc_id)
        final.append(result)

    return final
```

---

## Permission Filtering

```python
def filter_allowed_results(results, allowed_ids):
    allowed = set(allowed_ids)

    return [
        result
        for result in results
        if result["document_id"] in allowed
    ]
```

---

## Top-K Results

Sorting version:

```python
def top_k_results(results, k):
    if k <= 0:
        return []

    return sorted(
        results,
        key=lambda item: item["score"],
        reverse=True
    )[:k]
```

Heap version:

```python
import heapq

def top_k_results(results, k):
    if k <= 0:
        return []

    return heapq.nlargest(
        k,
        results,
        key=lambda item: item["score"]
    )
```

---

## Mini RAG Preprocessing Pipeline

```python
def normalize_text(text: str) -> str:
    return " ".join(
        text.strip().lower().split()
    )


def batch_items(items: list[str], batch_size: int) -> list[list[str]]:
    if batch_size <= 0:
        raise ValueError("batch_size must be positive")

    batches = []

    for start in range(0, len(items), batch_size):
        batches.append(items[start:start + batch_size])

    return batches


def rag_preprocess(chunks: list[str], batch_size: int) -> list[list[str]]:
    seen = set()
    unique_chunks = []

    for chunk in chunks:
        normalized = normalize_text(chunk)

        if not normalized:
            continue

        if normalized in seen:
            continue

        seen.add(normalized)
        unique_chunks.append(normalized)

    return batch_items(unique_chunks, batch_size)
```

---

# 12. Interview Answer Templates

## Complexity Answer

```text
The brute-force solution is O(n²) because it compares every pair.
We can optimize using a dictionary or set for O(1) average lookup.
The optimized solution is O(n) time and O(n) space.
This is a time-space tradeoff.
```

---

## Debugging Answer

```text
I would first reproduce the issue and read the traceback from the bottom.
Then I would identify the exception type, failing line, and variable values.
After that, I would create a minimal reproducible example, fix the root cause, and add a test or validation to prevent regression.
```

---

## Performance Answer

```text
I would measure before optimizing.
Then I would identify whether the bottleneck is CPU-bound, I/O-bound, memory-related, or caused by the wrong data structure.
After that, I would optimize the bottleneck and measure again.
```

---

## AI Engineering Answer

```text
I would break the pipeline into stages: normalization, deduplication, batching, embedding, storage, retrieval, reranking, and generation.
Then I would measure each stage separately.
For performance, I would focus on batching, caching, controlled concurrency, and avoiding duplicate work.
```

---

# 13. Rapid Revision Questions

Use these for quick self-testing.

## Memory

* What is the difference between variable and object?
* What is the difference between `==` and `is`?
* What is mutability?
* What is shallow copy?
* What is deep copy?
* Why are mutable defaults dangerous?

## Lists

* Why is `append()` O(1) amortized?
* Why is `insert(0, x)` O(n)?
* Why is `pop(0)` O(n)?
* What is slicing complexity?
* When should you avoid lists?

## Tuples

* Why are tuples useful?
* When can a tuple be a dictionary key?
* What is tuple unpacking?
* What is a single-element tuple?

## Sets

* Why is set membership O(1) average?
* Why can sets not contain lists?
* How do you deduplicate while preserving order?

## Dictionaries

* Why are dictionaries fast?
* What can be a dictionary key?
* How do you count frequencies?
* How do you group values?
* What is the risk of unbounded dictionary caches?

## Functions

* What is a closure?
* What are `*args` and `**kwargs`?
* What is a pure function?
* What is a side effect?
* How do you make a function testable?

## Complexity

* What is O(1)?
* What is O(n)?
* What is O(n²)?
* What is O(log n)?
* What is amortized complexity?
* What is a time-space tradeoff?

## Debugging

* How do you read a traceback?
* What causes KeyError?
* What causes IndexError?
* What causes AttributeError?
* Why avoid modifying a list while iterating?

## Performance

* What is CPU-bound?
* What is I/O-bound?
* When do you use generators?
* When do you use batching?
* When do you use caching?
* How do you profile Python code?

## AI Engineering

* Why normalize text before deduplication?
* How do you build an embedding cache key?
* How do you deduplicate retrieval results?
* How do you filter results by permissions?
* Why is batching important?

---

# 14. Final Core Python Mastery Checklist

You are ready to move to Module 02 — OOP when you can confidently:

* [ ] Explain Python variables, objects, and references
* [ ] Explain mutability and immutability
* [ ] Debug mutable default arguments
* [ ] Use lists correctly and explain their complexity
* [ ] Use tuples for fixed records and cache keys
* [ ] Use sets for fast membership and deduplication
* [ ] Use dictionaries for lookup, grouping, frequency counting, and caching
* [ ] Write clean functions with clear inputs and outputs
* [ ] Explain `*args`, `**kwargs`, closures, lambdas, and scope
* [ ] Analyze time and space complexity
* [ ] Move from brute force to optimized solutions
* [ ] Debug common Python errors
* [ ] Identify common performance bottlenecks
* [ ] Explain batching, caching, and deduplication in AI systems
* [ ] Complete all 50 exercises
* [ ] Review all 100 interview questions
* [ ] Complete the mock interview out loud

---

# Final Note

Core Python is not basic.

Core Python is the foundation for:

```text
OOP
Advanced Python
Concurrency
FastAPI
NumPy
Pandas
SQL workflows
ML coding
RAG systems
AI engineering
System design
```

A senior engineer does not only know syntax.

A senior engineer understands:

```text
behavior
memory
complexity
debugging
performance
tradeoffs
production impact
```

Master this module deeply before moving forward.
