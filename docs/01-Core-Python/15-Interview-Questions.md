# Core Python Interview Questions

> Interview questions and strong answers for Core Python: memory model, lists, tuples, sets, dictionaries, functions, complexity, debugging, performance, and AI engineering examples.

---

# How To Use This File

This file is designed for interview preparation.

Do not only read the answers.

For each question:

1. Try to answer out loud first.
2. Compare your answer with the expected answer.
3. Rewrite the answer in your own words.
4. Add a small code example.
5. Explain time and space complexity if relevant.

A strong interview answer usually includes:

```text
Concept
↓
Example
↓
Tradeoff
↓
Production relevance
```

---

# Interview Answer Framework

When answering technical questions, use this structure:

```text
1. Define the concept clearly.
2. Give a small Python example.
3. Explain why it matters.
4. Mention complexity or tradeoff if relevant.
5. Connect to production or AI engineering if useful.
```

Example:

```text
A dictionary is a mutable key-value data structure implemented using a hash table. It provides O(1) average lookup by key. It is useful for frequency counting, grouping, caching, and indexing records by ID. In AI systems, dictionaries commonly store metadata, retrieval results, and embedding cache entries.
```

---

# Section 1 — Python Memory Model

---

## Q1. What is the difference between a variable and an object in Python?

### Strong Answer

A variable is a name that references an object.

The object is the actual value stored in memory.

Example:

```python
x = [1, 2, 3]
```

Here:

```text
x -> reference -> list object [1, 2, 3]
```

The variable does not contain the list directly. It points to the list object.

This matters because assignment copies references, not objects.

---

## Q2. What happens here?

```python
a = [1, 2, 3]
b = a
b.append(4)

print(a)
```

### Strong Answer

Output:

```python
[1, 2, 3, 4]
```

Both `a` and `b` reference the same list object.

`b.append(4)` mutates that shared object, so `a` also sees the change.

---

## Q3. What is the difference between `==` and `is`?

### Strong Answer

`==` compares values.

`is` compares object identity.

Example:

```python
a = [1, 2]
b = [1, 2]

print(a == b)
print(a is b)
```

Output:

```python
True
False
```

The lists have the same values, but they are different objects in memory.

---

## Q4. What is mutability?

### Strong Answer

Mutability means whether an object can be changed after creation.

Mutable objects include:

```text
list
dict
set
```

Immutable objects include:

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
items = [1, 2]
items.append(3)
```

The list changes in place.

But:

```python
x = 10
x += 1
```

creates or references a new integer value because integers are immutable.

---

## Q5. Are tuples always fully immutable?

### Strong Answer

The tuple itself is immutable, meaning it cannot change which objects it references.

But if a tuple contains a mutable object, that inner object can still change.

Example:

```python
data = ([1, 2], "active")
data[0].append(3)

print(data)
```

Output:

```python
([1, 2, 3], 'active')
```

The tuple still references the same list, but the list changed internally.

---

## Q6. What is a shallow copy?

### Strong Answer

A shallow copy copies the outer container but keeps references to nested objects.

Example:

```python
import copy

a = [[1], [2]]
b = copy.copy(a)

b[0].append(99)

print(a)
```

Output:

```python
[[1, 99], [2]]
```

The outer list was copied, but the inner lists were shared.

---

## Q7. What is a deep copy?

### Strong Answer

A deep copy recursively copies nested objects.

Example:

```python
import copy

a = [[1], [2]]
b = copy.deepcopy(a)

b[0].append(99)

print(a)
print(b)
```

Output:

```python
[[1], [2]]
[[1, 99], [2]]
```

Use deep copy when nested mutable objects must be independent.

---

## Q8. What is the mutable default argument bug?

### Strong Answer

Default arguments are evaluated once when the function is defined.

Bad:

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

Calls:

```python
print(add_item("a"))
print(add_item("b"))
```

Output:

```python
['a']
['a', 'b']
```

The same list is reused.

Correct:

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

---

## Q9. How does Python manage memory?

### Strong Answer

In CPython, memory is primarily managed using reference counting.

When an object has no more references, it can be deallocated.

Python also has a garbage collector to handle reference cycles.

Example cycle:

```python
class Node:
    pass

a = Node()
b = Node()

a.next = b
b.next = a
```

Reference counting alone cannot clean cycles like this, so Python uses cyclic garbage collection.

---

## Q10. How can memory leaks happen in Python?

### Strong Answer

Python has automatic memory management, but memory growth can still happen when references are accidentally kept alive.

Common causes:

* unbounded dictionaries
* global lists
* caches without eviction
* background tasks holding references
* large objects stored unnecessarily
* circular references with cleanup problems

Example:

```python
cache = {}

def get_embedding(text):
    if text not in cache:
        cache[text] = embed(text)

    return cache[text]
```

This cache grows forever unless controlled.

---

# Section 2 — Lists

---

## Q11. What is a Python list?

### Strong Answer

A Python list is an ordered, mutable, dynamic collection.

It can store references to objects of different types.

Example:

```python
items = [1, "python", True]
```

Lists are implemented as dynamic arrays internally.

---

## Q12. How do Python lists work internally?

### Strong Answer

Python lists are dynamic arrays that store references to objects.

Conceptually:

```text
list slots -> references -> objects
```

This means the list does not store the raw objects directly. It stores pointers to objects.

This matters for memory, mutability, and performance.

---

## Q13. Why is `append()` O(1) amortized?

### Strong Answer

Python overallocates memory for lists.

Most `append()` operations simply place the new reference in available capacity, which is O(1).

Occasionally, when capacity is full, Python allocates a larger block and copies references, which is O(n).

Across many appends, the average cost is O(1) amortized.

---

## Q14. Why is `insert(0, x)` O(n)?

### Strong Answer

Inserting at the front requires shifting all existing elements one position to the right.

Example:

```python
nums = [1, 2, 3]
nums.insert(0, 100)
```

Result:

```python
[100, 1, 2, 3]
```

All existing elements moved, so complexity is O(n).

---

## Q15. Why is `pop()` from the end O(1), but `pop(0)` O(n)?

### Strong Answer

`pop()` removes the last element, so no shifting is required.

`pop(0)` removes the first element, so every remaining element must shift left.

Therefore:

```text
pop()   -> O(1)
pop(0) -> O(n)
```

Use `collections.deque` for efficient front removal.

---

## Q16. What is the complexity of list membership?

### Strong Answer

List membership is O(n).

Example:

```python
if x in items:
    ...
```

Python may need to scan every item until it finds a match.

If repeated membership checks are needed, a set is often better.

---

## Q17. What is the complexity of slicing?

### Strong Answer

Slicing creates a new list.

Example:

```python
copy_items = items[:]
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

Many candidates incorrectly think slicing is O(1), but it copies references into a new list.

---

## Q18. What is the difference between `sort()` and `sorted()`?

### Strong Answer

`list.sort()` modifies the list in place.

```python
nums.sort()
```

`sorted()` returns a new sorted list.

```python
new_nums = sorted(nums)
```

Use `sort()` when mutating the original list is acceptable.

Use `sorted()` when you need to preserve the original list.

---

## Q19. What is Timsort?

### Strong Answer

Timsort is Python's sorting algorithm.

It is stable and optimized for real-world partially sorted data.

Complexity:

```text
Best:    O(n)
Average: O(n log n)
Worst:   O(n log n)
```

Stable means equal elements keep their relative order.

---

## Q20. When should you avoid lists?

### Strong Answer

Avoid lists when:

* you need fast repeated membership checks: use `set`
* you need key-value lookup: use `dict`
* you need efficient front insert/remove: use `deque`
* you need numeric vectorized computation: use `NumPy`
* you need streaming: use generators

A senior engineer chooses the data structure based on the operation.

---

# Section 3 — Tuples

---

## Q21. What is a tuple?

### Strong Answer

A tuple is an ordered, immutable collection.

Example:

```python
point = (10, 20)
```

Tuples are useful for fixed records, multiple return values, and hashable composite keys.

---

## Q22. What is the difference between a list and a tuple?

### Strong Answer

A list is mutable and can grow or shrink.

A tuple is immutable and fixed after creation.

Use a list for changing collections.

Use a tuple for fixed groups of values.

---

## Q23. How do you create a single-element tuple?

### Strong Answer

Use a trailing comma:

```python
value = (10,)
```

Without the comma:

```python
value = (10)
```

Python treats it as an integer, not a tuple.

The comma creates the tuple.

---

## Q24. Why can tuples be dictionary keys?

### Strong Answer

Tuples can be dictionary keys if all their elements are hashable.

Example:

```python
key = ("user_123", "2026-06-02")
cache[key] = "result"
```

This works because strings are hashable.

But this does not work:

```python
key = ("user_123", ["read", "write"])
```

because the tuple contains a list, and lists are unhashable.

---

## Q25. What is tuple unpacking?

### Strong Answer

Tuple unpacking assigns tuple values to variables.

Example:

```python
point = (10, 20)
x, y = point
```

It is commonly used for:

* multiple return values
* swapping variables
* iterating pairs
* database rows
* search result tuples

---

## Q26. Where do tuples appear in AI systems?

### Strong Answer

Common examples:

```text
(document_id, score)
(label, probability)
(model_name, text_hash)
(user_id, item_id)
(feature_name, importance)
```

Tuples are useful when values naturally belong together and should not be modified.

---

# Section 4 — Sets

---

## Q27. What is a set?

### Strong Answer

A set is an unordered collection of unique hashable elements.

Example:

```python
items = {1, 2, 3}
```

Sets are useful for:

* fast membership checks
* deduplication
* tracking seen values
* mathematical set operations

---

## Q28. Why are sets fast for membership checks?

### Strong Answer

Sets use hash tables.

When checking:

```python
x in my_set
```

Python computes the hash of `x` and uses it to locate the value efficiently.

Average complexity:

```text
O(1)
```

Worst case can degrade due to hash collisions.

---

## Q29. Why can a set not contain a list?

### Strong Answer

Set elements must be hashable.

Lists are mutable and unhashable.

If a list changed after being placed in a set, its hash would change and the set would not know where to find it.

Use a tuple instead:

```python
items = {(1, 2), (3, 4)}
```

---

## Q30. How do you remove duplicates from a list?

### Strong Answer

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

This preserves the first occurrence.

---

## Q31. What is the difference between `remove()` and `discard()` in sets?

### Strong Answer

`remove()` raises an error if the item does not exist.

```python
items.remove(x)
```

`discard()` does nothing if the item does not exist.

```python
items.discard(x)
```

Use `discard()` when missing values are acceptable.

---

## Q32. What is a `frozenset`?

### Strong Answer

A `frozenset` is an immutable set.

Example:

```python
permissions = frozenset(["read", "write"])
```

Because it is immutable, it is hashable and can be used as a dictionary key or stored inside another set.

---

## Q33. Where are sets useful in RAG systems?

### Strong Answer

Sets are useful for:

* deduplicating chunks
* tracking retrieved document IDs
* avoiding repeated embedding calls
* permission filtering
* tracking processed files
* merging results from multiple retrievers

Example:

```python
seen_doc_ids = set()
```

---

# Section 5 — Dictionaries

---

## Q34. What is a dictionary?

### Strong Answer

A dictionary is a mutable key-value data structure.

Example:

```python
user = {
    "name": "Abubaker",
    "role": "Software Engineer"
}
```

It maps keys to values and provides O(1) average lookup by key.

---

## Q35. Why are dictionaries fast?

### Strong Answer

Dictionaries use hash tables.

Python computes the hash of a key and uses it to locate the value efficiently.

Average lookup, insert, update, and delete are O(1).

---

## Q36. What types can be dictionary keys?

### Strong Answer

Dictionary keys must be hashable.

Valid keys:

```text
str
int
float
bool
tuple of hashable values
frozenset
```

Invalid keys:

```text
list
dict
set
```

because they are mutable and unhashable.

---

## Q37. What is the difference between `dict[key]` and `dict.get(key)`?

### Strong Answer

`dict[key]` raises `KeyError` if the key is missing.

```python
user["email"]
```

`dict.get(key)` returns `None` or a provided default.

```python
user.get("email", "unknown")
```

Use `dict[key]` when the key is required.

Use `get()` when the key is optional or missing is expected.

---

## Q38. How do you count frequencies with a dictionary?

### Strong Answer

Use the `get()` pattern:

```python
freq = {}

for item in items:
    freq[item] = freq.get(item, 0) + 1
```

This is one of the most common interview patterns.

---

## Q39. How do you group items with a dictionary?

### Strong Answer

Use a dictionary of lists.

```python
groups = {}

for item in items:
    key = get_key(item)

    if key not in groups:
        groups[key] = []

    groups[key].append(item)
```

Or use `defaultdict(list)`.

---

## Q40. What is `defaultdict`?

### Strong Answer

`defaultdict` automatically creates a default value for missing keys.

Example:

```python
from collections import defaultdict

groups = defaultdict(list)

groups["python"].append("dict")
```

No manual initialization is required.

---

## Q41. What is `Counter`?

### Strong Answer

`Counter` is a dictionary subclass for counting hashable items.

Example:

```python
from collections import Counter

counts = Counter("banana")
```

Result:

```python
Counter({'a': 3, 'n': 2, 'b': 1})
```

Use it when frequency counting is the main goal.

---

## Q42. How do dictionaries appear in FastAPI and AI systems?

### Strong Answer

Dictionaries appear in:

* JSON payloads
* metadata
* configuration
* API responses
* caches
* model registries
* RAG search results
* document metadata
* feature maps

Example:

```python
result = {
    "document_id": "doc_123",
    "score": 0.91,
    "metadata": {
        "source": "core-python"
    }
}
```

---

## Q43. What is a common risk of dictionary caches?

### Strong Answer

An unbounded dictionary cache can grow forever in a long-running service.

This can cause:

* memory growth
* OOM crashes
* slow garbage collection
* unstable containers

Use:

* LRU cache
* TTL cache
* Redis
* Memcached
* persistent cache

---

# Section 6 — Functions

---

## Q44. What is a function?

### Strong Answer

A function is a reusable block of code that accepts inputs through parameters, performs logic, and optionally returns an output.

Example:

```python
def add(a, b):
    return a + b
```

Functions improve reuse, readability, testing, and maintainability.

---

## Q45. What is the difference between a parameter and an argument?

### Strong Answer

A parameter is defined in the function signature.

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

## Q46. What does a function return if there is no `return` statement?

### Strong Answer

Python returns `None`.

Example:

```python
def log_message():
    print("Hello")

result = log_message()
```

`result` is `None`.

---

## Q47. What are `*args`?

### Strong Answer

`*args` collects extra positional arguments into a tuple.

Example:

```python
def add_all(*numbers):
    return sum(numbers)
```

Inside the function, `numbers` is a tuple.

---

## Q48. What are `**kwargs`?

### Strong Answer

`**kwargs` collects extra keyword arguments into a dictionary.

Example:

```python
def create_profile(**kwargs):
    return kwargs
```

Inside the function, `kwargs` is a dictionary.

---

## Q49. What is a closure?

### Strong Answer

A closure is a function that remembers variables from its enclosing scope.

Example:

```python
def make_multiplier(factor):
    def multiply(value):
        return value * factor

    return multiply
```

`multiply()` remembers `factor`.

Closures are important for decorators, wrappers, configuration, and function factories.

---

## Q50. What is a lambda function?

### Strong Answer

A lambda is a small anonymous function.

Example:

```python
square = lambda x: x * x
```

Common use:

```python
users.sort(key=lambda user: user["age"])
```

Use lambda for short simple logic.

Use a named function for complex logic.

---

## Q51. What is a pure function?

### Strong Answer

A pure function returns the same output for the same input and has no side effects.

Example:

```python
def add(a, b):
    return a + b
```

Pure functions are easier to test, debug, cache, and parallelize.

---

## Q52. What is a side effect?

### Strong Answer

A side effect happens when a function changes something outside itself.

Examples:

* modifying a global variable
* mutating input arguments
* writing to a database
* writing a file
* printing
* making an API call

Side effects are sometimes necessary, but they should be intentional and clear.

---

## Q53. How do you make an API-calling function testable?

### Strong Answer

Use dependency injection.

Bad:

```python
def get_embedding(text):
    return openai_client.embed(text)
```

Better:

```python
def get_embedding(text, embedding_client):
    return embedding_client.embed(text)
```

Now tests can pass a fake client.

---

## Q54. How would you structure a RAG pipeline with functions?

### Strong Answer

Break it into small stages:

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

This improves testing, observability, debugging, and replacement of components.

---

# Section 7 — Complexity Analysis

---

## Q55. What is Big O?

### Strong Answer

Big O describes how runtime or memory usage grows as input size increases.

It does not measure exact seconds.

It describes growth behavior.

---

## Q56. What is O(1)?

### Strong Answer

O(1) means constant time.

The operation does not grow with input size.

Example:

```python
items[0]
```

Dictionary lookup is also O(1) average.

---

## Q57. What is O(n)?

### Strong Answer

O(n) means linear time.

Runtime grows proportionally with input size.

Example:

```python
for item in items:
    print(item)
```

---

## Q58. What is O(n²)?

### Strong Answer

O(n²) means quadratic time.

It often comes from nested loops over the same input.

Example:

```python
for a in items:
    for b in items:
        print(a, b)
```

---

## Q59. What is O(log n)?

### Strong Answer

O(log n) means the problem size is reduced by a constant factor each step.

Binary search is the classic example.

Requirement:

```text
The input must be sorted.
```

---

## Q60. What is space complexity?

### Strong Answer

Space complexity measures how much additional memory an algorithm uses as input grows.

Example:

```python
result = []
```

If `result` grows with input size, space complexity is O(n).

---

## Q61. What is the complexity of Two Sum brute force vs optimized?

### Strong Answer

Brute force checks all pairs:

```text
Time:  O(n²)
Space: O(1)
```

Optimized dictionary solution:

```text
Time:  O(n)
Space: O(n)
```

The optimized version uses extra memory for faster lookup.

---

## Q62. What is amortized complexity?

### Strong Answer

Amortized complexity describes average cost over many operations.

Example:

```python
items.append(x)
```

Most appends are O(1), but occasional resizing costs O(n).

Across many appends, the average cost is O(1) amortized.

---

## Q63. Why is Big O not enough for production performance?

### Strong Answer

Big O ignores constants and real-world bottlenecks.

Production performance also depends on:

* network latency
* database queries
* serialization
* API rate limits
* disk I/O
* memory layout
* external model latency
* Python interpreter overhead

In AI systems, model/API latency may dominate Python loop complexity.

---

# Section 8 — Debugging

---

## Q64. How do you debug a Python error?

### Strong Answer

I start by reading the traceback from the bottom.

I identify:

* exception type
* error message
* failing line
* input that caused it

Then I reproduce the issue with a small example, inspect variables, fix the root cause, and add a test if appropriate.

---

## Q65. What is a traceback?

### Strong Answer

A traceback is Python's error report showing the call stack that led to an exception.

It includes:

* file names
* line numbers
* function calls
* exception type
* error message

---

## Q66. What is the difference between `KeyError` and `IndexError`?

### Strong Answer

`KeyError` happens when a dictionary key is missing.

```python
user["email"]
```

`IndexError` happens when a list or tuple index is out of range.

```python
items[10]
```

---

## Q67. What causes `AttributeError: 'NoneType' object has no attribute ...`?

### Strong Answer

The code expected an object with an attribute or method, but received `None`.

Example:

```python
text = None
text.strip()
```

Fix by tracing where `None` came from and deciding whether to handle it or reject it.

---

## Q68. Why is modifying a list while iterating dangerous?

### Strong Answer

Because changing the list shifts elements while the loop is still processing it.

This can skip items or produce unexpected results.

Better:

```python
filtered = [x for x in items if condition(x)]
```

or build a new list.

---

## Q69. Why is modifying a dictionary while iterating dangerous?

### Strong Answer

Changing dictionary size during iteration can raise:

```text
RuntimeError: dictionary changed size during iteration
```

Better:

```python
cleaned = {
    key: value
    for key, value in data.items()
    if value is not None
}
```

---

## Q70. How would you debug a bad RAG answer?

### Strong Answer

I would debug the pipeline stage by stage:

1. user query
2. query normalization/rewrite
3. retrieved documents
4. scores
5. metadata filters
6. context sent to the LLM
7. prompt
8. model output

I would first check whether the correct documents were retrieved before blaming the model.

---

# Section 9 — Performance

---

## Q71. How do you approach Python performance optimization?

### Strong Answer

I first make the code correct and clear.

Then I measure before optimizing.

I identify whether the bottleneck is:

* CPU-bound
* I/O-bound
* memory-related
* data-structure-related
* external API-related

Then I optimize the bottleneck and measure again.

---

## Q72. What is CPU-bound work?

### Strong Answer

CPU-bound work spends most time using the processor.

Examples:

* numerical computation
* image processing
* compression
* large pure-Python loops
* parsing huge data

Possible solutions:

* better algorithm
* NumPy
* multiprocessing
* compiled libraries
* Numba/Cython

---

## Q73. What is I/O-bound work?

### Strong Answer

I/O-bound work spends most time waiting for external resources.

Examples:

* API calls
* database queries
* network requests
* file I/O
* object storage
* external AI services

Possible solutions:

* async I/O
* batching
* caching
* connection pooling
* retries
* timeouts
* concurrency

---

## Q74. Why is repeated list concatenation slow?

### Strong Answer

This is slow:

```python
result = result + [item]
```

because it creates a new list and copies existing elements every iteration.

Inside a loop, this can become O(n²).

Use:

```python
result.append(item)
```

or a list comprehension.

---

## Q75. When should you use generators?

### Strong Answer

Use generators when processing large data and you do not need all results in memory at once.

Generators produce values lazily.

This can reduce memory from O(n) to O(1) for streaming workflows.

---

## Q76. Why is batching important in AI systems?

### Strong Answer

Batching reduces overhead.

Instead of one API/model call per item:

```python
for text in texts:
    embed(text)
```

use:

```python
embed_batch(texts)
```

Batching improves throughput, reduces network overhead, and helps manage rate limits.

---

## Q77. How would you optimize embedding 100,000 documents?

### Strong Answer

I would:

* normalize text
* remove duplicates
* batch requests
* cache repeated inputs
* use retry/backoff
* control concurrency
* stream results to storage
* add metrics per stage
* avoid keeping unnecessary intermediate data in memory

---

## Q78. When should you use a heap instead of sorting?

### Strong Answer

Use sorting when simplicity is fine and input is moderate.

Use a heap when:

* input is large
* only top-k is needed
* k is much smaller than n
* data may be streamed

Sorting is O(n log n).

Heap top-k can be O(n log k).

---

## Q79. What tools can you use to measure Python performance?

### Strong Answer

Useful tools:

```text
timeit
time.perf_counter
cProfile
tracemalloc
logging
metrics
tracing
```

Use `timeit` for small snippets.

Use `cProfile` for function-level profiling.

Use `tracemalloc` for memory analysis.

Use logs/metrics/traces in production.

---

## Q80. What is premature optimization?

### Strong Answer

Premature optimization means making code more complex before proving there is a performance problem.

It can make systems harder to read, test, and maintain.

A better approach:

```text
Write clear code
Measure
Identify bottleneck
Optimize only what matters
Measure again
```

---

# Section 10 — AI Engineering Core Python Questions

---

## Q81. How does Core Python apply to AI engineering?

### Strong Answer

Core Python is used in AI engineering for:

* preprocessing text
* deduplicating chunks
* batching embedding requests
* managing metadata
* building cache keys
* filtering retrieval results
* ranking results
* building pipelines
* debugging payloads
* optimizing service performance

AI engineering is not only models. It is also production Python around models.

---

## Q82. How would you build a safe embedding cache key?

### Strong Answer

A safe embedding cache key should include:

* model name
* embedding version
* normalized text hash
* tenant/user scope if needed

Example:

```python
key = (
    model_name,
    embedding_version,
    text_hash
)
```

This prevents mixing embeddings across models or versions.

---

## Q83. Why normalize text before deduplication?

### Strong Answer

Without normalization, these look different:

```text
"Python Functions"
" python   functions "
"PYTHON FUNCTIONS"
```

But they should probably be treated as the same chunk.

Normalization may include:

* stripping spaces
* lowercasing
* collapsing repeated spaces

This reduces duplicate embeddings and improves indexing quality.

---

## Q84. How would you deduplicate retrieval results?

### Strong Answer

Use a set of stable document or chunk IDs.

```python
seen = set()
final = []

for result in results:
    doc_id = result["document_id"]

    if doc_id in seen:
        continue

    seen.add(doc_id)
    final.append(result)
```

This preserves first occurrence and gives O(n) time.

---

## Q85. How would you filter search results by permissions efficiently?

### Strong Answer

Convert allowed IDs to a set.

```python
allowed = set(allowed_ids)

visible = [
    result
    for result in results
    if result["document_id"] in allowed
]
```

This changes repeated lookup from O(n) list search to O(1) average set lookup.

---

## Q86. How would you structure a preprocessing pipeline?

### Strong Answer

Split it into small functions:

```python
def normalize_text(text):
    ...

def remove_empty(chunks):
    ...

def deduplicate(chunks):
    ...

def batch_items(items, batch_size):
    ...
```

Then compose them.

This improves testing, debugging, and observability.

---

## Q87. What is the risk of storing millions of embeddings in a Python list?

### Strong Answer

Risks include:

* high memory usage
* no efficient similarity search
* poor persistence
* difficult scaling
* slow filtering/searching
* container memory pressure

Better options:

* NumPy arrays
* FAISS
* vector database
* PostgreSQL pgvector
* object storage plus index

---

## Q88. How do dictionaries appear in RAG pipelines?

### Strong Answer

Dictionaries are used for:

* document metadata
* retrieval results
* cache entries
* API responses
* config
* model registries

Example:

```python
{
    "document_id": "doc_1",
    "score": 0.92,
    "text": "...",
    "metadata": {
        "source": "docs"
    }
}
```

---

## Q89. How do sets appear in RAG pipelines?

### Strong Answer

Sets are used for:

* deduplicating chunks
* tracking seen document IDs
* permission filtering
* avoiding repeated processing
* merging retriever outputs

They are important because membership is O(1) average.

---

## Q90. What Core Python skills are most important for AI engineers?

### Strong Answer

The most important Core Python skills are:

* dictionaries
* sets
* functions
* list processing
* complexity analysis
* debugging
* performance thinking
* batching
* caching
* clean data transformations

These are used daily in production AI systems.

---

# Section 11 — Rapid Fire Questions

---

## Q91. List vs set for membership?

### Answer

Use set for repeated membership checks.

List membership is O(n).

Set membership is O(1) average.

---

## Q92. List vs dict?

### Answer

Use list for ordered collections.

Use dict for key-value lookup.

---

## Q93. Tuple vs list?

### Answer

Use tuple for fixed immutable records.

Use list for mutable collections.

---

## Q94. `append()` vs `extend()`?

### Answer

`append()` adds one item.

`extend()` adds all items from an iterable.

```python
a.append([1, 2])  # adds list as one item
a.extend([1, 2])  # adds 1 and 2 separately
```

---

## Q95. `remove()` vs `pop()`?

### Answer

`remove(value)` removes by value.

`pop(index)` removes by index and returns the removed item.

---

## Q96. `copy.copy()` vs `copy.deepcopy()`?

### Answer

`copy.copy()` makes a shallow copy.

`copy.deepcopy()` recursively copies nested objects.

---

## Q97. `is None` vs `== None`?

### Answer

Use:

```python
x is None
```

`None` is a singleton, so identity comparison is the correct style.

---

## Q98. Why should you avoid bare `except Exception`?

### Answer

It can hide real bugs.

Catch specific exceptions when possible.

Bad:

```python
except Exception:
    pass
```

Better:

```python
except ValueError:
    ...
```

---

## Q99. Why use type hints?

### Answer

Type hints improve readability, tooling, autocomplete, static analysis, and maintainability.

They do not enforce runtime types by default.

---

## Q100. What makes a Python solution senior-level?

### Answer

A senior-level solution is:

* correct
* readable
* tested against edge cases
* efficient enough
* explained clearly
* aware of tradeoffs
* production-conscious

It does not only pass the sample input.

It explains why the approach is appropriate.

---

# Final Interview Checklist

Before a Python interview, confirm you can explain:

* [ ] variables vs objects
* [ ] references
* [ ] mutability
* [ ] shallow vs deep copy
* [ ] list complexity
* [ ] tuple hashability
* [ ] set lookup
* [ ] dictionary lookup
* [ ] function defaults
* [ ] closures
* [ ] Big O
* [ ] debugging tracebacks
* [ ] performance bottlenecks
* [ ] caching
* [ ] batching
* [ ] RAG preprocessing basics

---

# How To Practice These Questions

Use this routine:

```text
Day 1:
Memory model + lists

Day 2:
Tuples + sets + dictionaries

Day 3:
Functions + complexity

Day 4:
Debugging + performance

Day 5:
AI engineering questions

Day 6:
Mock interview

Day 7:
Review weak areas
```

Repeat until you can answer naturally without memorizing word-for-word.

---

# Final Note

Interviewers are not looking for memorized definitions.

They are looking for evidence that you can reason.

Strong candidates say:

```text
The brute-force solution works, but it is O(n²). I can improve it with a dictionary to get O(n) time at the cost of O(n) extra space.
```

That is the level expected from Senior Software Engineers and AI Engineers.
