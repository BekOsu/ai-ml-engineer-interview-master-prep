# Python Performance

> Practical performance thinking for Python interviews, backend services, data pipelines, and AI engineering systems.

---

# Why Performance Matters

Performance is not only about writing faster code.

Performance is about understanding:

* where time is spent
* where memory is used
* which operations are expensive
* which data structures fit the problem
* when optimization is worth it
* when optimization is a distraction

Senior engineers are expected to answer:

```text
Why is this code slow?
How would you measure it?
What is the bottleneck?
How would you improve it?
What tradeoff are you making?
```

For AI Engineer roles, Python performance matters in:

* RAG preprocessing
* chunking
* embedding pipelines
* vector search post-processing
* data cleaning
* batch inference
* FastAPI services
* API fanout
* retry logic
* caching
* serialization
* Pandas and NumPy workflows

This chapter focuses on practical performance, not micro-optimization trivia.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain the difference between performance and complexity
* Identify common slow Python patterns
* Use the correct data structure for performance
* Avoid unnecessary copies
* Understand CPU-bound vs I/O-bound work
* Use `timeit` for small benchmarks
* Use `cProfile` for function-level profiling
* Use `tracemalloc` for memory debugging
* Explain when to use generators
* Explain when to use batching
* Explain when to use caching
* Optimize Python code safely
* Discuss performance tradeoffs in interviews
* Apply performance thinking to AI systems

---

# Performance vs Complexity

Complexity analysis answers:

```text
How does this grow as input grows?
```

Performance analysis answers:

```text
Where is the actual time or memory going?
```

Example:

```python
for doc in documents:
    embedding = call_embedding_api(doc)
```

Big O:

```text
O(n)
```

But the real bottleneck may be:

```text
network latency
external API response time
rate limits
serialization
```

So the best optimization may not be changing the loop.

It may be:

* batching
* caching
* async calls
* retries with backoff
* request compression
* better timeout handling

Senior engineers understand both algorithmic complexity and real-world bottlenecks.

---

# The Performance Mindset

Do not optimize randomly.

Use this process:

```text
1. Make it correct
2. Make it clear
3. Measure it
4. Identify bottleneck
5. Optimize the bottleneck
6. Measure again
7. Keep the code maintainable
```

Bad approach:

```text
I think this part is slow, so I will rewrite everything.
```

Good approach:

```text
I measured the pipeline and found 80% of time is spent in embedding API calls, so I will batch requests and cache repeated inputs.
```

---

# Golden Rule

Do not guess.

Measure.

---

# Common Performance Bottlenecks In Python

Common sources of slowness:

* wrong data structure
* repeated list membership checks
* repeated list concatenation
* unnecessary copying
* nested loops
* excessive function calls in hot loops
* repeated sorting
* repeated API calls
* loading everything into memory
* processing one item at a time
* no caching
* inefficient string concatenation
* using pure Python loops for numeric computation
* blocking I/O
* excessive logging
* JSON serialization/deserialization overhead

---

# CPU-Bound vs I/O-Bound

This distinction is extremely important.

## CPU-Bound

CPU-bound work spends most time using the processor.

Examples:

* image processing
* cryptographic hashing
* large pure-Python loops
* numerical computation
* compression
* parsing huge files
* ML preprocessing in Python loops

Possible solutions:

* better algorithm
* NumPy vectorization
* multiprocessing
* compiled libraries
* Cython/Numba
* move heavy work outside Python

---

## I/O-Bound

I/O-bound work spends most time waiting.

Examples:

* API calls
* database queries
* file reads/writes
* network requests
* object storage downloads
* calls to external AI services

Possible solutions:

* async I/O
* concurrency
* batching
* caching
* connection pooling
* timeouts
* retries
* queues

---

# Interview Answer Template

When asked:

```text
How would you improve performance?
```

A strong answer is:

```text
First, I would measure before optimizing. I would identify whether the bottleneck is CPU-bound, I/O-bound, memory-related, or caused by the wrong data structure. Then I would optimize the bottleneck with the smallest safe change and measure again.
```

---

# Data Structure Choice

Choosing the wrong data structure is one of the most common performance mistakes.

---

## List Membership

Bad for repeated lookup:

```python
allowed_ids = ["u1", "u2", "u3"]

if user_id in allowed_ids:
    process(user_id)
```

Complexity:

```text
O(n)
```

Better:

```python
allowed_ids = {"u1", "u2", "u3"}

if user_id in allowed_ids:
    process(user_id)
```

Average complexity:

```text
O(1)
```

---

## Real Example

Bad:

```python
def filter_documents(documents, allowed_ids):
    result = []

    for doc in documents:
        if doc["id"] in allowed_ids:
            result.append(doc)

    return result
```

If `allowed_ids` is a list:

```text
O(n * m)
```

Better:

```python
def filter_documents(documents, allowed_ids):
    allowed = set(allowed_ids)

    return [
        doc
        for doc in documents
        if doc["id"] in allowed
    ]
```

Complexity:

```text
O(n + m)
```

---

# Avoid Repeated List Concatenation

Bad:

```python
result = []

for item in items:
    result = result + [item]
```

Why slow?

Each iteration creates a new list.

For many items, this becomes very expensive.

Better:

```python
result = []

for item in items:
    result.append(item)
```

Even better when transforming:

```python
result = [
    transform(item)
    for item in items
]
```

---

# Avoid String Concatenation In Loops

Bad:

```python
text = ""

for word in words:
    text += word + " "
```

This repeatedly creates new strings.

Better:

```python
text = " ".join(words)
```

Why?

Strings are immutable.

Each concatenation creates a new string.

---

# Avoid Repeated Work

Bad:

```python
def process_users(users):
    result = []

    for user in users:
        active_users = get_active_users()

        if user in active_users:
            result.append(user)

    return result
```

Problem:

```python
get_active_users()
```

is called repeatedly.

Better:

```python
def process_users(users):
    active_users = set(get_active_users())

    result = []

    for user in users:
        if user in active_users:
            result.append(user)

    return result
```

Move repeated work outside the loop.

---

# Avoid Repeated Sorting

Bad:

```python
for query in queries:
    results.sort(key=lambda item: item["score"], reverse=True)
    top = results[:5]
```

If results do not change, sorting repeatedly is wasteful.

Better:

```python
sorted_results = sorted(
    results,
    key=lambda item: item["score"],
    reverse=True
)

for query in queries:
    top = sorted_results[:5]
```

---

# Avoid Unnecessary Copies

Slicing creates a new list.

```python
copy_items = items[:]
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

This is fine when needed.

But avoid accidental copies in hot paths.

Bad:

```python
for i in range(len(items)):
    window = items[i:i+100]
    process(window)
```

Each slice creates a new list.

For large inputs, this can be expensive.

---

# Use Generators For Streaming

A generator produces items lazily.

Example:

```python
def read_lines(lines):
    for line in lines:
        yield line.strip()
```

Instead of creating all results at once:

```python
cleaned = [
    line.strip()
    for line in lines
]
```

Use a generator when:

* input is large
* you do not need all results at once
* you want streaming behavior
* memory matters

---

# Generator Example

```python
def normalize_texts(texts):
    for text in texts:
        yield " ".join(text.strip().lower().split())
```

Usage:

```python
for text in normalize_texts(large_texts):
    process(text)
```

Memory:

```text
O(1) extra memory
```

Instead of:

```text
O(n)
```

for building a full list.

---

# List Comprehension vs Generator Expression

List comprehension:

```python
squares = [x * x for x in nums]
```

Creates a full list.

Generator expression:

```python
squares = (x * x for x in nums)
```

Creates lazy generator.

Use list comprehension when:

* you need all results
* data is reasonably sized
* you need indexing or repeated iteration

Use generator expression when:

* data is large
* one-pass processing is enough
* memory matters

---

# Example: Sum Squares

List version:

```python
total = sum([x * x for x in nums])
```

Generator version:

```python
total = sum(x * x for x in nums)
```

The generator version avoids creating an intermediate list.

---

# Batching

Batching is one of the most important AI engineering performance patterns.

Bad:

```python
embeddings = []

for text in texts:
    embeddings.append(embed(text))
```

This may call the model/API once per text.

Better:

```python
embeddings = embed_batch(texts)
```

Even better for large inputs:

```python
def batch_items(items, batch_size):
    for start in range(0, len(items), batch_size):
        yield items[start:start + batch_size]
```

Usage:

```python
all_embeddings = []

for batch in batch_items(texts, batch_size=64):
    embeddings = embed_batch(batch)
    all_embeddings.extend(embeddings)
```

Benefits:

* fewer API calls
* better throughput
* lower overhead
* easier rate-limit management

---

# Caching

Caching stores expensive results for reuse.

Example:

```python
cache = {}

def get_embedding(text):
    if text in cache:
        return cache[text]

    embedding = embed(text)
    cache[text] = embedding

    return embedding
```

This avoids repeated embedding calls.

But this cache is unbounded.

In production, prefer:

* LRU cache
* TTL cache
* Redis
* database-backed cache
* vector database
* disk cache

---

# LRU Cache

Python provides `functools.lru_cache`.

```python
from functools import lru_cache

@lru_cache(maxsize=1000)
def expensive_square(x):
    return x * x
```

This keeps only the most recent 1000 cached results.

Important:

Arguments must be hashable.

---

# Bad Cache Key

Bad:

```python
cache[text] = embedding
```

Problem:

Same text may be embedded by different models.

Better:

```python
key = (model_name, embedding_version, text_hash)
cache[key] = embedding
```

---

# Building Safe Cache Keys

```python
import hashlib

def normalize_text(text: str) -> str:
    return " ".join(text.strip().lower().split())

def build_text_hash(text: str) -> str:
    normalized = normalize_text(text)

    return hashlib.sha256(
        normalized.encode("utf-8")
    ).hexdigest()

def build_embedding_cache_key(
    model_name: str,
    embedding_version: str,
    text: str
) -> tuple[str, str, str]:
    return (
        model_name,
        embedding_version,
        build_text_hash(text)
    )
```

---

# Measuring Small Code With timeit

`timeit` is useful for small benchmarks.

Example:

```python
import timeit

duration = timeit.timeit(
    stmt="[x * x for x in range(1000)]",
    number=1000
)

print(duration)
```

Use `timeit` for comparing small snippets.

Do not use it as the only tool for production performance.

---

# Comparing List vs Set Membership

```python
import timeit

setup = """
items_list = list(range(100000))
items_set = set(items_list)
target = 99999
"""

list_time = timeit.timeit(
    stmt="target in items_list",
    setup=setup,
    number=1000
)

set_time = timeit.timeit(
    stmt="target in items_set",
    setup=setup,
    number=1000
)

print("list:", list_time)
print("set:", set_time)
```

Expected result:

```text
set membership should be much faster for repeated lookup
```

The exact numbers depend on machine and environment.

---

# Measuring Functions With time.perf_counter

For larger blocks, use `time.perf_counter()`.

```python
import time

start = time.perf_counter()

run_pipeline()

end = time.perf_counter()

print(f"Duration: {end - start:.4f} seconds")
```

Use this for rough timing around real functions.

---

# Profiling With cProfile

`cProfile` shows where time is spent.

Example:

```python
import cProfile

def main():
    run_pipeline()

cProfile.run("main()")
```

This helps answer:

```text
Which functions are taking the most time?
```

Use profiling before rewriting code.

---

# Example Profiling Target

```python
def slow_deduplicate(items):
    result = []

    for item in items:
        if item not in result:
            result.append(item)

    return result
```

Problem:

```text
item not in result
```

is O(n).

Optimized:

```python
def fast_deduplicate(items):
    seen = set()
    result = []

    for item in items:
        if item in seen:
            continue

        seen.add(item)
        result.append(item)

    return result
```

---

# Memory Debugging With tracemalloc

`tracemalloc` helps identify memory allocation.

Example:

```python
import tracemalloc

tracemalloc.start()

run_pipeline()

current, peak = tracemalloc.get_traced_memory()

print(f"Current memory: {current / 1024 / 1024:.2f} MB")
print(f"Peak memory: {peak / 1024 / 1024:.2f} MB")

tracemalloc.stop()
```

Useful when debugging:

* memory growth
* large intermediate lists
* unbounded caches
* large payload processing
* batch size issues

---

# Performance Pattern: Process In Batches

Bad:

```python
all_results = process_all(items)
```

if this loads everything into memory.

Better:

```python
for batch in batch_items(items, batch_size=1000):
    results = process_batch(batch)
    save_results(results)
```

Benefits:

* lower memory
* better failure isolation
* easier retry
* better monitoring
* easier scaling

---

# Performance Pattern: Stream Large Files

Bad:

```python
content = file.read()
lines = content.splitlines()
```

This loads everything into memory.

Better:

```python
with open("large_file.txt", "r") as file:
    for line in file:
        process(line)
```

This processes line by line.

---

# Performance Pattern: Use Built-ins

Python built-ins are usually faster than manual loops because many are implemented efficiently.

Prefer:

```python
total = sum(nums)
```

over:

```python
total = 0

for num in nums:
    total += num
```

Prefer:

```python
maximum = max(nums)
```

over manual max when no custom logic is needed.

Prefer:

```python
any(x > 10 for x in nums)
```

over manual flag loops.

But in interviews, you may be asked to implement from scratch.

Know both.

---

# Performance Pattern: Use collections

The `collections` module provides optimized tools.

## Counter

```python
from collections import Counter

counts = Counter(items)
```

## defaultdict

```python
from collections import defaultdict

groups = defaultdict(list)

for item in items:
    groups[key(item)].append(item)
```

## deque

```python
from collections import deque

queue = deque()
queue.append(item)
queue.popleft()
```

Use `deque` instead of `list.pop(0)`.

---

# deque vs list.pop(0)

Bad:

```python
queue = []

queue.pop(0)
```

Complexity:

```text
O(n)
```

Better:

```python
from collections import deque

queue = deque()
queue.popleft()
```

Complexity:

```text
O(1)
```

Useful for:

* BFS
* task queues
* event processing
* streaming pipelines

---

# Numeric Performance: Use NumPy

Python lists are not ideal for heavy numerical computation.

Bad:

```python
result = []

for x in values:
    result.append(x * 2)
```

For large numeric arrays, use NumPy:

```python
import numpy as np

arr = np.array(values)
result = arr * 2
```

Why faster?

* contiguous memory
* fixed data type
* vectorized operations
* optimized low-level implementation

This will be covered deeply in the NumPy module.

---

# Pandas Performance Preview

Bad Pandas pattern:

```python
df["new"] = df["value"].apply(lambda x: x * 2)
```

Better:

```python
df["new"] = df["value"] * 2
```

Vectorized operations are usually faster than row-wise `apply`.

This will be covered in the Pandas module.

---

# JSON Serialization Cost

Backend and AI systems often spend time serializing and deserializing JSON.

Example:

```python
import json

payload = json.dumps(data)
data = json.loads(payload)
```

For large payloads, this cost can matter.

Performance questions:

* Can payload size be reduced?
* Are unnecessary fields included?
* Can data be streamed?
* Can binary formats be used?
* Is serialization happening repeatedly?

---

# Logging Performance

Logging can become expensive.

Bad:

```python
logger.info(f"Large payload: {payload}")
```

This builds the string even if the log level disables it.

Better:

```python
logger.info("Large payload: %s", payload)
```

Also avoid logging huge payloads in hot paths.

Especially avoid logging:

* full prompts
* full documents
* large embeddings
* secrets
* tokens
* personal data

---

# AI Performance Example: Embedding Pipeline

Naive:

```python
def embed_documents(documents):
    embeddings = []

    for doc in documents:
        embedding = embed(doc["text"])
        embeddings.append({
            "id": doc["id"],
            "embedding": embedding
        })

    return embeddings
```

Problems:

* one API/model call per document
* no batching
* no caching
* no retry strategy
* loads all results in memory

Better:

```python
def batch_items(items, batch_size):
    for start in range(0, len(items), batch_size):
        yield items[start:start + batch_size]

def embed_documents(documents, batch_size=64):
    results = []

    for batch in batch_items(documents, batch_size):
        texts = [doc["text"] for doc in batch]
        embeddings = embed_batch(texts)

        for doc, embedding in zip(batch, embeddings):
            results.append({
                "id": doc["id"],
                "embedding": embedding
            })

    return results
```

Further production improvements:

* stream results to storage
* cache repeated text
* retry failed batches
* add metrics
* use async or queue workers
* limit memory usage

---

# AI Performance Example: RAG Retrieval Deduplication

Bad:

```python
final_results = []

for result in all_results:
    ids = [item["document_id"] for item in final_results]

    if result["document_id"] not in ids:
        final_results.append(result)
```

Problems:

* builds `ids` repeatedly
* membership check on list
* O(n²)

Better:

```python
def deduplicate_results(results):
    seen_ids = set()
    final_results = []

    for result in results:
        doc_id = result["document_id"]

        if doc_id in seen_ids:
            continue

        seen_ids.add(doc_id)
        final_results.append(result)

    return final_results
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# AI Performance Example: Top-K Results

Bad for very large inputs:

```python
sorted_results = sorted(
    results,
    key=lambda item: item["score"],
    reverse=True
)

top_k = sorted_results[:k]
```

Complexity:

```text
O(n log n)
```

Better for large `n` and small `k`:

```python
import heapq

top_k = heapq.nlargest(
    k,
    results,
    key=lambda item: item["score"]
)
```

Complexity:

```text
O(n log k)
```

Tradeoff:

Sorting is simpler.

Heap is better when `n` is large and `k` is small.

---

# Backend Performance Example: Handler Lookup

Bad:

```python
def handle_event(event):
    if event["type"] == "invoice.created":
        return handle_invoice_created(event)
    elif event["type"] == "payment.completed":
        return handle_payment_completed(event)
    elif event["type"] == "user.created":
        return handle_user_created(event)
```

Better:

```python
HANDLERS = {
    "invoice.created": handle_invoice_created,
    "payment.completed": handle_payment_completed,
    "user.created": handle_user_created
}

def handle_event(event):
    handler = HANDLERS.get(event["type"])

    if handler is None:
        raise ValueError(f"Unknown event type: {event['type']}")

    return handler(event)
```

Benefits:

* cleaner
* faster lookup
* easier extension
* easier testing

---

# Premature Optimization

Do not make code complex before needed.

Bad mindset:

```text
I will optimize everything immediately.
```

Better mindset:

```text
I will write clean code, measure bottlenecks, then optimize the parts that matter.
```

Premature optimization can cause:

* unreadable code
* more bugs
* harder testing
* wrong assumptions
* wasted time

But ignoring performance completely is also bad.

Balance is the senior skill.

---

# Common Performance Mistakes

## Mistake 1: Using List For Repeated Membership

Bad:

```python
if item in huge_list:
    ...
```

Better:

```python
huge_set = set(huge_list)

if item in huge_set:
    ...
```

---

## Mistake 2: Repeated Concatenation

Bad:

```python
result = result + [item]
```

Better:

```python
result.append(item)
```

---

## Mistake 3: Loading Everything Into Memory

Bad:

```python
all_rows = load_all_rows()
process(all_rows)
```

Better:

```python
for batch in load_rows_in_batches():
    process(batch)
```

---

## Mistake 4: API Call Inside Tight Loop

Bad:

```python
for item in items:
    call_api(item)
```

Better:

```python
for batch in batch_items(items, 50):
    call_batch_api(batch)
```

---

## Mistake 5: No Cache For Expensive Repeated Work

Bad:

```python
for text in texts:
    embedding = embed(text)
```

even when many texts repeat.

Better:

```python
cache = {}

for text in texts:
    key = normalize_text(text)

    if key not in cache:
        cache[key] = embed(text)

    embedding = cache[key]
```

---

## Mistake 6: Optimizing Without Measuring

Bad:

```text
I think this is slow.
```

Better:

```text
I measured this function and it consumes 70% of runtime.
```

---

# Performance Review Checklist

When reviewing code, ask:

## Algorithm

* Is complexity acceptable?
* Is there a nested loop?
* Is repeated lookup using a list?
* Is sorting repeated unnecessarily?

## Data Structures

* Should this be a set?
* Should this be a dictionary?
* Should this be a deque?
* Should this be NumPy/Pandas?

## Memory

* Are we copying large lists?
* Are we loading everything at once?
* Is the cache unbounded?
* Can we stream or batch?

## I/O

* Are there API calls inside loops?
* Can requests be batched?
* Can requests be async?
* Are timeouts configured?
* Is retry logic safe?

## Production

* Are we logging too much?
* Are we serializing large payloads repeatedly?
* Do we have metrics?
* Can we observe the bottleneck?

---

# Interview Questions And Answers

## Q1. How do you approach Python performance optimization?

I start by measuring before optimizing. I identify whether the bottleneck is algorithmic, CPU-bound, I/O-bound, memory-related, or caused by the wrong data structure. Then I optimize the bottleneck and measure again.

---

## Q2. Why is using a set faster than a list for membership checks?

A list scans elements sequentially, so membership is O(n).

A set uses a hash table, so membership is O(1) average.

---

## Q3. Why is repeated list concatenation slow?

Because each concatenation creates a new list and copies existing elements.

Inside a loop, this can become O(n²).

Use `append()` or list comprehensions instead.

---

## Q4. When should you use a generator?

Use a generator when data is large and you do not need all results in memory at once.

Generators produce values lazily and can reduce memory usage.

---

## Q5. What is the difference between CPU-bound and I/O-bound work?

CPU-bound work is limited by processor computation.

I/O-bound work is limited by waiting for external resources like network, disk, database, or APIs.

Different bottlenecks require different solutions.

---

## Q6. How would you optimize repeated API calls?

I would consider:

* batching
* caching
* async calls
* retries with backoff
* rate limiting
* connection reuse
* avoiding duplicate calls

---

## Q7. How would you profile Python code?

For small snippets, use `timeit`.

For larger functions, use `time.perf_counter()` or `cProfile`.

For memory, use `tracemalloc`.

In production, use logs, metrics, and tracing.

---

## Q8. Why can NumPy be faster than Python lists?

NumPy uses contiguous memory, fixed data types, and optimized vectorized operations.

Python lists store references to Python objects, which adds overhead.

---

## Q9. What is premature optimization?

Premature optimization means making code more complex before proving there is a performance problem.

It can reduce readability and introduce bugs.

The better approach is to write clear code, measure, and optimize real bottlenecks.

---

## Q10. How do you optimize a slow RAG preprocessing pipeline?

I would inspect each step:

* text normalization
* deduplication
* chunking
* embedding calls
* vector DB writes

Then I would optimize bottlenecks using batching, caching, sets for deduplication, streaming, and metrics.

---

# Senior-Level Questions And Answers

## Senior Q1. A FastAPI endpoint is slow. How would you debug performance?

I would measure the endpoint end to end and break it into stages:

```text
request validation
database query
external API calls
business logic
serialization
response time
```

Then I would identify the slowest stage.

If the bottleneck is database, I would inspect query plans and indexes.

If it is external API, I would consider batching, async, timeouts, retries, and caching.

If it is CPU-bound Python code, I would profile and optimize the algorithm or move heavy work to a worker.

---

## Senior Q2. A container memory keeps increasing. What do you check?

I would check:

* unbounded dictionaries
* global lists
* caches without eviction
* large batches
* loading full files into memory
* references retained by background tasks
* repeated copies
* large logs or payloads
* memory leaks in dependencies

Tools:

```python
tracemalloc
gc
memory_profiler
```

Also check metrics over time.

---

## Senior Q3. How would you design an embedding pipeline for performance?

I would design it with:

* text normalization
* deduplication
* batching
* caching
* retry/backoff
* controlled concurrency
* streaming output
* metrics per batch
* failure handling
* memory limits

I would avoid one API call per document and avoid storing unnecessary intermediate data.

---

## Senior Q4. When would you choose heap over sorting?

Use sorting when simplicity matters and data size is moderate.

Use heap when:

* data is large
* only top-k is needed
* k is much smaller than n
* streaming top-k is needed

Sorting is O(n log n).

Heap top-k can be O(n log k).

---

## Senior Q5. How do you balance readability and performance?

I prefer readable code first.

If profiling proves a bottleneck, I optimize that section while keeping the design understandable.

I avoid clever code that saves tiny time but makes the system harder to maintain.

For senior work, maintainability is part of performance because bugs and slow iteration are expensive.

---

## Senior Q6. How would you optimize a Python loop doing heavy numeric computation?

Options:

* replace pure Python loop with NumPy vectorization
* use Pandas vectorized operations
* use compiled libraries
* use multiprocessing for CPU-bound work
* use Numba/Cython if appropriate
* change algorithm if complexity is poor

First, I would measure to confirm the loop is the bottleneck.

---

## Senior Q7. How do you reason about performance in AI systems?

I separate costs into:

```text
Python preprocessing
model inference
network/API latency
retrieval/index lookup
reranking
prompt construction
LLM generation
serialization
storage
```

Then I measure each stage.

Often the biggest bottleneck is not Python code but model/API latency or inefficient retrieval design.

---

# Exercises

## Exercise 1 — Optimize Membership

Improve this:

```python
def filter_allowed(users, allowed_ids):
    result = []

    for user in users:
        if user["id"] in allowed_ids:
            result.append(user)

    return result
```

Assume `allowed_ids` is a large list.

---

## Exercise 2 — Fix Repeated Concatenation

Improve this:

```python
def collect(items):
    result = []

    for item in items:
        result = result + [item]

    return result
```

---

## Exercise 3 — Use Generator

Rewrite this to avoid creating a full list:

```python
def normalize_all(texts):
    return [
        text.strip().lower()
        for text in texts
    ]
```

---

## Exercise 4 — Batch Items

Implement:

```python
def batch_items(items, batch_size):
    pass
```

---

## Exercise 5 — Cache Expensive Function

Implement a dictionary cache for:

```python
def expensive_square(x):
    return x * x
```

---

## Exercise 6 — Safe Embedding Cache Key

Build a cache key using:

```text
model_name
version
normalized_text_hash
```

---

## Exercise 7 — Top K With Heap

Given search results:

```python
results = [
    {"id": "doc1", "score": 0.8},
    {"id": "doc2", "score": 0.95},
    {"id": "doc3", "score": 0.7},
]
```

Return top `k` by score.

---

## Exercise 8 — Deduplicate Results

Deduplicate results by `document_id`.

---

## Exercise 9 — Measure With perf_counter

Write a function wrapper that measures execution time.

---

## Exercise 10 — Stream File Lines

Write code that processes a file line by line.

---

## Exercise 11 — Replace pop(0)

Improve this queue:

```python
queue = []

while queue:
    item = queue.pop(0)
```

---

## Exercise 12 — Identify Bottleneck

Given:

```python
for doc in docs:
    cleaned = normalize(doc)
    embedding = call_embedding_api(cleaned)
    save_embedding(doc["id"], embedding)
```

Explain likely bottlenecks and improvements.

---

# Solutions

## Solution 1

```python
def filter_allowed(users, allowed_ids):
    allowed = set(allowed_ids)

    result = []

    for user in users:
        if user["id"] in allowed:
            result.append(user)

    return result
```

Or:

```python
def filter_allowed(users, allowed_ids):
    allowed = set(allowed_ids)

    return [
        user
        for user in users
        if user["id"] in allowed
    ]
```

---

## Solution 2

```python
def collect(items):
    result = []

    for item in items:
        result.append(item)

    return result
```

Or simply:

```python
def collect(items):
    return list(items)
```

---

## Solution 3

```python
def normalize_all(texts):
    for text in texts:
        yield text.strip().lower()
```

Usage:

```python
for text in normalize_all(texts):
    process(text)
```

---

## Solution 4

```python
def batch_items(items, batch_size):
    if batch_size <= 0:
        raise ValueError("batch_size must be positive")

    for start in range(0, len(items), batch_size):
        yield items[start:start + batch_size]
```

---

## Solution 5

```python
cache = {}

def expensive_square(x):
    if x in cache:
        return cache[x]

    result = x * x
    cache[x] = result

    return result
```

---

## Solution 6

```python
import hashlib

def normalize_text(text):
    return " ".join(text.strip().lower().split())

def build_embedding_cache_key(model_name, version, text):
    normalized = normalize_text(text)

    text_hash = hashlib.sha256(
        normalized.encode("utf-8")
    ).hexdigest()

    return model_name, version, text_hash
```

---

## Solution 7

```python
import heapq

def top_k_results(results, k):
    return heapq.nlargest(
        k,
        results,
        key=lambda item: item["score"]
    )
```

---

## Solution 8

```python
def deduplicate_by_document_id(results):
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

## Solution 9

```python
import time
from collections.abc import Callable
from typing import TypeVar

T = TypeVar("T")

def measure_time(func: Callable[[], T]) -> T:
    start = time.perf_counter()

    result = func()

    end = time.perf_counter()

    print(f"Duration: {end - start:.4f} seconds")

    return result
```

Usage:

```python
result = measure_time(lambda: run_pipeline())
```

---

## Solution 10

```python
def process_file(path):
    with open(path, "r") as file:
        for line in file:
            process(line.strip())
```

This avoids loading the full file into memory.

---

## Solution 11

```python
from collections import deque

queue = deque()

while queue:
    item = queue.popleft()
```

---

## Solution 12

Likely bottlenecks:

```text
call_embedding_api(cleaned)
save_embedding(...)
```

because these are I/O-bound operations.

Improvements:

* batch embedding calls
* cache repeated text
* use retries and timeouts
* save in batches
* use async or worker queue
* add metrics per stage
* avoid duplicate documents
* control concurrency to respect rate limits

---

# Mock Interview

## Interviewer

How do you optimize Python code?

## Strong Candidate Answer

I first make sure the code is correct and clear. Then I measure it to identify the bottleneck. Depending on the bottleneck, I may improve the algorithm, change data structures, reduce copies, batch I/O, add caching, use generators, or move CPU-heavy work to optimized libraries.

---

## Interviewer

Why is this slow?

```python
for item in items:
    result = result + [item]
```

## Strong Candidate Answer

Each iteration creates a new list and copies previous elements. Over many iterations this can become O(n²). Using `append()` avoids repeated copying and gives O(n) total time.

---

## Interviewer

How do you improve repeated membership checks?

## Strong Candidate Answer

If membership is checked repeatedly against a list, I convert the list to a set once. This changes lookup from O(n) to O(1) average.

---

## Interviewer

When would you use a generator?

## Strong Candidate Answer

I would use a generator when processing large data where I do not need all results in memory at once. It helps reduce memory usage and supports streaming pipelines.

---

## Interviewer

How would you speed up embedding 100,000 documents?

## Strong Candidate Answer

I would avoid one call per document. I would normalize and deduplicate text, batch embedding calls, cache repeated inputs, control concurrency, add retries, and stream results to storage instead of keeping everything in memory.

---

## Interviewer

How do you know whether to use threading, async, or multiprocessing?

## Strong Candidate Answer

It depends on the bottleneck. For I/O-bound work like API calls, async or threading can help. For CPU-bound work, multiprocessing or optimized libraries are usually better because CPU-heavy Python code is limited by interpreter overhead and the GIL.

---

# Revision Sheet

## Performance Process

```text
Correct
↓
Clear
↓
Measure
↓
Find bottleneck
↓
Optimize
↓
Measure again
```

---

## Common Fixes

| Problem                 | Fix            |
| ----------------------- | -------------- |
| Repeated list lookup    | Convert to set |
| Repeated list concat    | Use append     |
| Large intermediate list | Use generator  |
| One API call per item   | Batch calls    |
| Repeated expensive work | Cache          |
| Queue with pop(0)       | Use deque      |
| Heavy numeric loop      | Use NumPy      |
| Top-k from huge list    | Use heap       |
| Large file loaded fully | Stream lines   |

---

## Tools

| Tool                | Use                      |
| ------------------- | ------------------------ |
| `timeit`            | small benchmarks         |
| `time.perf_counter` | timing code blocks       |
| `cProfile`          | function-level profiling |
| `tracemalloc`       | memory tracking          |
| logs/metrics/traces | production diagnosis     |

---

## Data Structure Reminders

```text
List membership: O(n)
Set membership: O(1) average
Dict lookup:     O(1) average
Deque popleft:   O(1)
List pop(0):     O(n)
```

---

# Cheat Sheet

## Timing

```python
import time

start = time.perf_counter()
run()
end = time.perf_counter()

print(end - start)
```

---

## Benchmark

```python
import timeit

timeit.timeit("x in items", setup="items=set(range(1000)); x=999")
```

---

## Profile

```python
import cProfile

cProfile.run("main()")
```

---

## Memory

```python
import tracemalloc

tracemalloc.start()
run()
current, peak = tracemalloc.get_traced_memory()
tracemalloc.stop()
```

---

## Batch

```python
for start in range(0, len(items), batch_size):
    batch = items[start:start + batch_size]
```

---

## Generator

```python
def stream_items(items):
    for item in items:
        yield process(item)
```

---

## Cache

```python
if key not in cache:
    cache[key] = expensive_call()

return cache[key]
```

---

## Top K

```python
heapq.nlargest(k, items, key=lambda x: x["score"])
```

---

# Final Key Takeaways

Python performance is about tradeoffs.

The best senior-level answer is not:

```text
Use faster code.
```

The best answer is:

```text
Measure first, identify the bottleneck, optimize the right layer, and keep the code maintainable.
```

For AI engineering, the bottleneck is often not Python itself.

It may be:

* embedding API latency
* LLM generation time
* vector database retrieval
* reranking
* serialization
* network I/O
* poor batching
* missing cache

Performance optimization is a system-level skill.

Master these patterns before moving to the exercise sets.
