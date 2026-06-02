# Complexity Analysis

> Time complexity, space complexity, Big O notation, tradeoff analysis, and senior-level performance reasoning.

---

# Why Complexity Analysis Matters

Complexity analysis is one of the most important interview skills.

Interviewers do not only care whether your code works.

They care whether you can answer:

* How fast is this solution?
* How much memory does it use?
* Will it scale?
* What happens with 1 million inputs?
* Can you improve it?
* Why is this solution better than the brute-force solution?

For Senior Software Engineer and AI Engineer roles, complexity analysis appears in:

* Coding interviews
* Backend performance reviews
* Data pipeline optimization
* API latency debugging
* RAG retrieval optimization
* Batch processing
* ML feature engineering
* Vector search
* Database query design

A senior engineer must be able to reason about performance before production fails.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain Big O notation
* Analyze time complexity
* Analyze space complexity
* Compare brute-force and optimized solutions
* Identify nested-loop performance issues
* Understand constant, linear, logarithmic, quadratic, and exponential complexity
* Choose the right data structure based on complexity
* Explain tradeoffs clearly in interviews
* Analyze real AI/backend examples

---

# What Is Complexity Analysis?

Complexity analysis is the process of estimating how an algorithm behaves as input size grows.

We usually care about:

```text
n = input size
```

Example:

```python
def print_items(items):
    for item in items:
        print(item)
```

If `items` has:

```text
10 elements   -> 10 operations
100 elements  -> 100 operations
1000 elements -> 1000 operations
```

This grows linearly.

So we say:

```text
O(n)
```

---

# What Is Big O?

Big O describes the upper bound of growth.

It answers:

```text
How does runtime or memory grow as input grows?
```

It does not measure exact seconds.

It describes growth behavior.

Example:

```python
def get_first(items):
    return items[0]
```

No matter how large the list is, this function does one access.

Complexity:

```text
O(1)
```

---

# Common Big O Classes

| Complexity | Name         | Example              |
| ---------- | ------------ | -------------------- |
| O(1)       | Constant     | Access list by index |
| O(log n)   | Logarithmic  | Binary search        |
| O(n)       | Linear       | Loop over list       |
| O(n log n) | Linearithmic | Sorting              |
| O(n²)      | Quadratic    | Nested loops         |
| O(2ⁿ)      | Exponential  | Naive recursion      |
| O(n!)      | Factorial    | Permutations         |

---

# O(1) — Constant Time

A function is O(1) if runtime does not grow with input size.

Example:

```python
def get_first(items):
    return items[0]
```

Whether the list has 10 items or 10 million items, accessing index `0` is constant time.

Other examples:

```python
items.append(x)
```

Average:

```text
O(1) amortized
```

Dictionary lookup:

```python
user = users[user_id]
```

Average:

```text
O(1)
```

Set membership:

```python
if user_id in active_users:
    ...
```

Average:

```text
O(1)
```

---

# O(n) — Linear Time

A function is O(n) if runtime grows linearly with input size.

Example:

```python
def find_item(items, target):
    for item in items:
        if item == target:
            return True

    return False
```

Worst case:

```text
Target is last item or missing.
```

Complexity:

```text
O(n)
```

---

# O(n²) — Quadratic Time

Nested loops often produce O(n²).

Example:

```python
def print_pairs(items):
    for a in items:
        for b in items:
            print(a, b)
```

If `items` has 100 elements:

```text
100 * 100 = 10,000 operations
```

If `items` has 10,000 elements:

```text
10,000 * 10,000 = 100,000,000 operations
```

Complexity:

```text
O(n²)
```

This is often unacceptable for large inputs.

---

# O(log n) — Logarithmic Time

Logarithmic algorithms reduce the problem size each step.

Classic example:

```text
Binary Search
```

Instead of checking every item, binary search cuts the search space in half.

Example:

```python
def binary_search(nums, target):
    left = 0
    right = len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

Complexity:

```text
O(log n)
```

Important requirement:

```text
Input must be sorted.
```

---

# O(n log n)

Common in efficient sorting algorithms.

Python sorting:

```python
sorted(items)
```

Average/worst complexity:

```text
O(n log n)
```

Many interview problems require sorting first, then scanning.

Example:

```python
def sort_then_scan(nums):
    nums.sort()

    for num in nums:
        print(num)
```

Complexity:

```text
Sorting: O(n log n)
Loop:    O(n)

Total:   O(n log n)
```

The larger term dominates.

---

# O(2ⁿ) — Exponential Time

Exponential algorithms grow very quickly.

Example: naive Fibonacci.

```python
def fib(n):
    if n <= 1:
        return n

    return fib(n - 1) + fib(n - 2)
```

This recalculates the same values many times.

Complexity:

```text
O(2ⁿ)
```

This is usually too slow for large `n`.

---

# Dropping Constants

Big O ignores constants.

Example:

```python
def print_twice(items):
    for item in items:
        print(item)

    for item in items:
        print(item)
```

Operations:

```text
2n
```

Big O:

```text
O(n)
```

We drop constant factors.

Why?

Because Big O focuses on growth rate.

---

# Dropping Lower-Order Terms

Example:

```python
def example(items):
    for item in items:
        print(item)

    for a in items:
        for b in items:
            print(a, b)
```

Complexity:

```text
O(n + n²)
```

The dominant term is:

```text
O(n²)
```

So final complexity:

```text
O(n²)
```

---

# Different Inputs

Be careful when there are two input sizes.

Example:

```python
def print_pairs(a, b):
    for x in a:
        for y in b:
            print(x, y)
```

Complexity is not:

```text
O(n²)
```

unless both lists are the same size.

Correct:

```text
O(a * b)
```

or:

```text
O(n * m)
```

---

# Time Complexity vs Space Complexity

## Time Complexity

How runtime grows.

Example:

```text
How many operations?
```

## Space Complexity

How memory usage grows.

Example:

```text
How much extra memory?
```

A solution can be fast but memory-heavy.

Another solution can be slower but memory-efficient.

Senior engineers explain both.

---

# Space Complexity Example

```python
def copy_items(items):
    result = []

    for item in items:
        result.append(item)

    return result
```

Time:

```text
O(n)
```

Space:

```text
O(n)
```

because it creates a new list.

---

# O(1) Space Example

```python
def sum_items(items):
    total = 0

    for item in items:
        total += item

    return total
```

Time:

```text
O(n)
```

Space:

```text
O(1)
```

because only one variable is used.

---

# Complexity Of Core Python Data Structures

## Lists

| Operation       | Complexity     |
| --------------- | -------------- |
| Access by index | O(1)           |
| Append          | O(1) amortized |
| Insert at front | O(n)           |
| Search          | O(n)           |
| Membership      | O(n)           |
| Sort            | O(n log n)     |

---

## Tuples

| Operation       | Complexity |
| --------------- | ---------- |
| Access by index | O(1)       |
| Iteration       | O(n)       |
| Search          | O(n)       |
| Hashing         | O(n)       |

---

## Sets

| Operation    | Complexity             |
| ------------ | ---------------------- |
| Add          | O(1) average           |
| Remove       | O(1) average           |
| Membership   | O(1) average           |
| Iteration    | O(n)                   |
| Union        | O(len(a) + len(b))     |
| Intersection | O(min(len(a), len(b))) |

---

## Dictionaries

| Operation     | Complexity   |
| ------------- | ------------ |
| Lookup by key | O(1) average |
| Insert        | O(1) average |
| Update        | O(1) average |
| Delete        | O(1) average |
| Iterate       | O(n)         |

---

# Common Interview Pattern: Brute Force To Optimized

Interviewers like to see progression.

Start with brute force.

Then optimize.

---

# Example: Two Sum Brute Force

Problem:

Given:

```python
nums = [2, 7, 11, 15]
target = 9
```

Return indexes of two numbers that add to target.

Brute force:

```python
def two_sum_brute(nums, target):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] + nums[j] == target:
                return [i, j]

    return []
```

Time:

```text
O(n²)
```

Space:

```text
O(1)
```

---

# Two Sum Optimized

Use dictionary.

```python
def two_sum(nums, target):
    seen = {}

    for i, num in enumerate(nums):
        diff = target - num

        if diff in seen:
            return [seen[diff], i]

        seen[num] = i

    return []
```

Time:

```text
O(n)
```

Space:

```text
O(n)
```

Tradeoff:

```text
More memory
↓
Faster runtime
```

This is a classic senior-level explanation.

---

# Example: Duplicate Detection

Brute force:

```python
def has_duplicates_brute(nums):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] == nums[j]:
                return True

    return False
```

Time:

```text
O(n²)
```

Space:

```text
O(1)
```

Optimized:

```python
def has_duplicates(nums):
    seen = set()

    for num in nums:
        if num in seen:
            return True

        seen.add(num)

    return False
```

Time:

```text
O(n)
```

Space:

```text
O(n)
```

---

# Example: Membership Check

Bad:

```python
allowed_ids = ["u1", "u2", "u3"]

for user_id in incoming_user_ids:
    if user_id in allowed_ids:
        process(user_id)
```

If:

```text
incoming_user_ids = n
allowed_ids = m
```

Complexity:

```text
O(n * m)
```

Better:

```python
allowed_set = set(allowed_ids)

for user_id in incoming_user_ids:
    if user_id in allowed_set:
        process(user_id)
```

Complexity:

```text
O(m + n)
```

This is a major optimization.

---

# Production AI Example: Deduplicating Chunks

Bad:

```python
unique_chunks = []

for chunk in chunks:
    if chunk not in unique_chunks:
        unique_chunks.append(chunk)
```

Complexity:

```text
O(n²)
```

Because `chunk in unique_chunks` scans the list.

Better:

```python
seen = set()
unique_chunks = []

for chunk in chunks:
    key = chunk.strip().lower()

    if key in seen:
        continue

    seen.add(key)
    unique_chunks.append(chunk)
```

Complexity:

```text
O(n)
```

This can save serious time in RAG pipelines.

---

# Production AI Example: Vector Search Results

Suppose you combine results from:

* vector search
* keyword search
* reranker
* metadata filter

Bad deduplication:

```python
final_results = []

for result in all_results:
    if result["document_id"] not in [
        item["document_id"] for item in final_results
    ]:
        final_results.append(result)
```

This repeatedly builds lists and scans them.

Better:

```python
seen_doc_ids = set()
final_results = []

for result in all_results:
    doc_id = result["document_id"]

    if doc_id in seen_doc_ids:
        continue

    seen_doc_ids.add(doc_id)
    final_results.append(result)
```

Time:

```text
O(n)
```

Space:

```text
O(n)
```

---

# Production Backend Example: API Filtering

Bad:

```python
def filter_allowed_documents(results, allowed_ids):
    filtered = []

    for result in results:
        if result["id"] in allowed_ids:
            filtered.append(result)

    return filtered
```

If `allowed_ids` is a list:

```text
O(n * m)
```

Better:

```python
def filter_allowed_documents(results, allowed_ids):
    allowed = set(allowed_ids)

    return [
        result
        for result in results
        if result["id"] in allowed
    ]
```

Complexity:

```text
O(n + m)
```

---

# Nested Loops Are Not Always O(n²)

Important interview nuance.

Example:

```python
def print_pairs(a, b):
    for x in a:
        for y in b:
            print(x, y)
```

If:

```text
len(a) = n
len(b) = m
```

Complexity:

```text
O(n * m)
```

Not always O(n²).

Only O(n²) if both sizes are approximately equal.

---

# Sequential Loops Are Added

Example:

```python
def process(items):
    for item in items:
        print(item)

    for item in items:
        print(item)

    for item in items:
        print(item)
```

Operations:

```text
n + n + n = 3n
```

Big O:

```text
O(n)
```

---

# Nested Loops Are Multiplied

Example:

```python
def nested(items):
    for a in items:
        for b in items:
            print(a, b)
```

Operations:

```text
n * n
```

Big O:

```text
O(n²)
```

---

# Sorting Plus Loop

Example:

```python
def sort_and_process(items):
    items.sort()

    for item in items:
        print(item)
```

Complexity:

```text
O(n log n + n)
```

Final:

```text
O(n log n)
```

Sorting dominates.

---

# Recursion Complexity

Recursive functions require careful analysis.

Example:

```python
def factorial(n):
    if n <= 1:
        return 1

    return n * factorial(n - 1)
```

Time:

```text
O(n)
```

Space:

```text
O(n)
```

Why space O(n)?

Because each recursive call uses stack space.

---

# Binary Search Complexity

```python
def binary_search(nums, target):
    left = 0
    right = len(nums) - 1

    while left <= right:
        mid = (left + right) // 2

        if nums[mid] == target:
            return mid

        if nums[mid] < target:
            left = mid + 1
        else:
            right = mid - 1

    return -1
```

Each step halves the search space.

Time:

```text
O(log n)
```

Space:

```text
O(1)
```

---

# Complexity Tradeoffs

Optimization usually involves tradeoffs.

Example:

```text
Use dictionary
```

Pros:

* Faster lookup
* Cleaner algorithms

Cons:

* More memory
* Hashing overhead
* Not ordered by numeric index

Example:

```text
Use sorting
```

Pros:

* Enables two-pointer solutions
* Reduces some problems from O(n²) to O(n log n)

Cons:

* Changes order
* Costs O(n log n)
* May require extra copy if original order must be preserved

Senior engineers discuss tradeoffs, not only final Big O.

---

# Common Complexity Mistakes

## Mistake 1: Thinking All Loops Are O(n)

Nested loops can be O(n²), O(n*m), or worse.

---

## Mistake 2: Forgetting Slicing Creates Copies

```python
items[:]
```

Complexity:

```text
O(n)
```

Not O(1).

---

## Mistake 3: Ignoring `in` On Lists

```python
if x in my_list:
    ...
```

Complexity:

```text
O(n)
```

If repeated inside a loop, this may become O(n²).

---

## Mistake 4: Ignoring Space Complexity

A solution may be fast but memory-heavy.

Example:

```python
seen = set(nums)
```

Space:

```text
O(n)
```

---

## Mistake 5: Confusing Average And Worst Case

Dictionary and set lookups are O(1) average.

Worst case can degrade due to collisions.

In interviews, say:

```text
O(1) average case
```

---

# Senior Deep Dive: Amortized Complexity

Some operations are usually cheap but occasionally expensive.

Example:

```python
items.append(x)
```

Most appends:

```text
O(1)
```

Occasional resize:

```text
O(n)
```

Average over many appends:

```text
O(1) amortized
```

This is called amortized complexity.

Strong interview answer:

```text
Python list append is O(1) amortized because Python overallocates memory, so most appends do not resize the list.
```

---

# Senior Deep Dive: Real Performance vs Big O

Big O is important, but real performance also depends on:

* Constants
* Memory layout
* CPU cache
* Network latency
* Disk I/O
* Database queries
* Serialization
* Python interpreter overhead
* External API calls

Example:

```python
for text in texts:
    embedding = call_embedding_api(text)
```

Big O:

```text
O(n)
```

But real bottleneck is likely:

```text
network/API latency
```

Optimization may require:

* batching
* async calls
* caching
* retry logic
* rate limiting

not only algorithm changes.

---

# Senior Deep Dive: AI Systems Complexity

In AI systems, complexity often combines:

```text
data size
model cost
network latency
storage lookup
retrieval depth
reranking cost
```

Example RAG pipeline:

```text
Query
↓
Embed query
↓
Retrieve top-k
↓
Rerank candidates
↓
Build prompt
↓
Call LLM
```

Each step has a cost.

Example:

```text
Embedding:       O(model cost)
Vector search:   depends on index
Reranking:       O(k * model cost)
Prompt building: O(total context size)
LLM generation:  O(tokens generated)
```

Senior AI engineers reason about the whole system, not only Python loops.

---

# How To Explain Complexity In Interviews

Use this structure:

```text
1. State brute force approach
2. Analyze brute force time and space
3. Identify bottleneck
4. Propose optimized approach
5. Analyze optimized time and space
6. Explain tradeoff
```

Example:

```text
The brute force solution checks all pairs, so time is O(n²) and space is O(1). We can optimize using a dictionary to store previously seen numbers. That gives O(n) time but uses O(n) extra space. This is a classic time-space tradeoff.
```

This is exactly how strong candidates sound.

---

# Interview Questions And Answers

## Q1. What is Big O?

Big O describes how an algorithm's runtime or memory usage grows as input size increases.

It focuses on growth rate, not exact runtime.

---

## Q2. What is O(1)?

O(1) means constant time.

The operation does not grow with input size.

Example:

```python
items[0]
```

---

## Q3. What is O(n)?

O(n) means linear time.

Runtime grows proportionally with input size.

Example:

```python
for item in items:
    print(item)
```

---

## Q4. What is O(n²)?

O(n²) means quadratic time.

It often comes from nested loops over the same input.

Example:

```python
for a in items:
    for b in items:
        print(a, b)
```

---

## Q5. What is O(log n)?

O(log n) means the problem size is reduced by a constant factor each step.

Binary search is the classic example.

---

## Q6. What is space complexity?

Space complexity measures how much extra memory an algorithm uses as input grows.

---

## Q7. What is the complexity of dictionary lookup?

Average case:

```text
O(1)
```

Worst case can degrade because of hash collisions.

---

## Q8. What is the complexity of list membership?

```python
x in my_list
```

Complexity:

```text
O(n)
```

because Python may need to scan the list.

---

## Q9. What is the complexity of set membership?

Average case:

```text
O(1)
```

because sets use hash tables.

---

## Q10. What is amortized O(1)?

It means that although some individual operations may be expensive, the average cost over many operations is constant.

Example:

```python
list.append()
```

---

# Senior-Level Questions And Answers

## Senior Q1. Why is Big O not enough for production performance?

Big O ignores constants and external factors.

In production, performance may depend on:

* network latency
* database query plans
* serialization
* memory layout
* API rate limits
* CPU cache
* disk I/O

A theoretically efficient algorithm may still be slow due to real-world bottlenecks.

---

## Senior Q2. How would you optimize a slow RAG deduplication step?

First identify the bottleneck.

If deduplication uses list membership:

```python
if doc_id in final_doc_ids:
    ...
```

convert `final_doc_ids` to a set.

This changes repeated lookup from O(n) to O(1) average.

Also normalize IDs consistently before comparison.

---

## Senior Q3. When is O(n log n) better than O(n²)?

For large inputs, O(n log n) is much better.

Sorting-based solutions often improve brute-force pair comparisons.

Example:

* brute force pair comparison: O(n²)
* sort + two pointers: O(n log n)

---

## Senior Q4. How do you discuss time-space tradeoffs?

A common pattern:

```text
Use extra memory to reduce runtime.
```

Example:

Two Sum:

* brute force: O(n²) time, O(1) space
* dictionary: O(n) time, O(n) space

The optimized version is faster but uses more memory.

---

## Senior Q5. How would you analyze an API pipeline?

Break it into stages.

Example:

```text
validate input
query database
call external service
transform response
serialize output
```

Analyze each stage separately.

The slowest stage usually dominates.

For AI systems, model/API latency may dominate Python computation.

---

## Senior Q6. Why can a simple O(n) loop be slow in AI applications?

Because each loop iteration may call an external model API.

Example:

```python
for doc in documents:
    embed(doc)
```

Big O is O(n), but real cost is:

```text
n * API latency
```

Optimization may require batching or concurrency.

---

# Exercises

## Exercise 1 — Analyze Complexity

What is the time complexity?

```python
def example(items):
    for item in items:
        print(item)
```

---

## Exercise 2 — Analyze Nested Loop

```python
def example(items):
    for a in items:
        for b in items:
            print(a, b)
```

---

## Exercise 3 — Analyze Different Inputs

```python
def example(a, b):
    for x in a:
        for y in b:
            print(x, y)
```

---

## Exercise 4 — Analyze Dictionary Lookup

```python
def get_user(users, user_id):
    return users.get(user_id)
```

Assume `users` is a dictionary.

---

## Exercise 5 — Analyze List Membership

```python
def contains(items, target):
    return target in items
```

Assume `items` is a list.

---

## Exercise 6 — Optimize Duplicate Detection

Write brute-force and optimized duplicate detection.

---

## Exercise 7 — Optimize Permission Filtering

Given:

```python
allowed_ids = [...]
documents = [...]
```

Optimize filtering documents by allowed IDs.

---

## Exercise 8 — Analyze Sorting

```python
def process(items):
    items.sort()

    for item in items:
        print(item)
```

---

## Exercise 9 — Analyze Space

```python
def copy_items(items):
    result = []

    for item in items:
        result.append(item)

    return result
```

---

## Exercise 10 — Analyze Binary Search

What are the time and space complexities of iterative binary search?

---

## Exercise 11 — AI Deduplication

Optimize this code:

```python
unique_chunks = []

for chunk in chunks:
    if chunk not in unique_chunks:
        unique_chunks.append(chunk)
```

---

## Exercise 12 — Two Sum

Explain brute-force and optimized complexity.

---

# Solutions

## Solution 1

Time:

```text
O(n)
```

One loop over all items.

Space:

```text
O(1)
```

No extra growing data structure.

---

## Solution 2

Time:

```text
O(n²)
```

Nested loop over the same input.

Space:

```text
O(1)
```

---

## Solution 3

Time:

```text
O(n * m)
```

where:

```text
n = len(a)
m = len(b)
```

Space:

```text
O(1)
```

---

## Solution 4

Dictionary lookup average:

```text
O(1)
```

Space:

```text
O(1)
```

for the lookup itself.

---

## Solution 5

List membership:

```text
O(n)
```

Worst case scans the full list.

---

## Solution 6

Brute force:

```python
def has_duplicates_brute(nums):
    for i in range(len(nums)):
        for j in range(i + 1, len(nums)):
            if nums[i] == nums[j]:
                return True

    return False
```

Time:

```text
O(n²)
```

Space:

```text
O(1)
```

Optimized:

```python
def has_duplicates(nums):
    seen = set()

    for num in nums:
        if num in seen:
            return True

        seen.add(num)

    return False
```

Time:

```text
O(n)
```

Space:

```text
O(n)
```

---

## Solution 7

Optimized:

```python
def filter_documents(documents, allowed_ids):
    allowed = set(allowed_ids)

    return [
        doc
        for doc in documents
        if doc["id"] in allowed
    ]
```

If:

```text
n = number of documents
m = number of allowed IDs
```

Complexity:

```text
O(n + m)
```

Space:

```text
O(m)
```

---

## Solution 8

Sorting:

```text
O(n log n)
```

Loop:

```text
O(n)
```

Total:

```text
O(n log n)
```

Space depends on implementation, but from an interview perspective focus on time unless asked.

---

## Solution 9

Time:

```text
O(n)
```

Space:

```text
O(n)
```

because a new result list is created.

---

## Solution 10

Iterative binary search:

Time:

```text
O(log n)
```

Space:

```text
O(1)
```

---

## Solution 11

Optimized:

```python
def deduplicate_chunks(chunks):
    seen = set()
    unique_chunks = []

    for chunk in chunks:
        key = chunk.strip().lower()

        if key in seen:
            continue

        seen.add(key)
        unique_chunks.append(chunk)

    return unique_chunks
```

Time:

```text
O(n)
```

Space:

```text
O(n)
```

---

## Solution 12

Brute force Two Sum:

```text
Time:  O(n²)
Space: O(1)
```

Optimized dictionary solution:

```text
Time:  O(n)
Space: O(n)
```

Tradeoff:

```text
Use extra memory to reduce runtime.
```

---

# Mock Interview

## Interviewer

What is Big O?

## Strong Candidate Answer

Big O describes how runtime or memory usage grows as input size increases. It does not measure exact seconds; it describes growth behavior.

---

## Interviewer

What is the complexity of this?

```python
for item in items:
    print(item)
```

## Strong Candidate Answer

Time complexity is O(n) because we visit every item once. Space complexity is O(1) because we do not allocate extra memory that grows with input size.

---

## Interviewer

What about this?

```python
for a in items:
    for b in items:
        print(a, b)
```

## Strong Candidate Answer

Time complexity is O(n²) because for each item we loop over all items again. Space complexity is O(1), assuming printing does not store results.

---

## Interviewer

How can you optimize duplicate detection?

## Strong Candidate Answer

The brute-force solution compares every pair, which is O(n²). We can use a set to track seen values. Each lookup is O(1) average, so total time becomes O(n), with O(n) extra space.

---

## Interviewer

How do you explain time-space tradeoff?

## Strong Candidate Answer

Sometimes we use extra memory to reduce runtime. For example, in Two Sum, using a dictionary reduces time from O(n²) to O(n), but uses O(n) additional space.

---

## Interviewer

In an AI system, is Big O enough?

## Strong Candidate Answer

No. Big O is useful, but AI systems often involve external API calls, model inference, network latency, token generation, vector search, and storage. The full system cost must be analyzed stage by stage.

---

# Revision Sheet

## Common Complexities

| Complexity | Meaning      |
| ---------- | ------------ |
| O(1)       | Constant     |
| O(log n)   | Logarithmic  |
| O(n)       | Linear       |
| O(n log n) | Sorting-like |
| O(n²)      | Nested loops |
| O(2ⁿ)      | Exponential  |

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

| Structure         | Lookup       |
| ----------------- | ------------ |
| List membership   | O(n)         |
| Set membership    | O(1) average |
| Dictionary lookup | O(1) average |
| List index access | O(1)         |

---

## Common Tradeoff

```text
Faster runtime
usually costs
more memory
```

Example:

```text
Dictionary or set optimization
```

---

# Cheat Sheet

| Pattern              | Brute Force | Optimized             |
| -------------------- | ----------- | --------------------- |
| Duplicate detection  | O(n²)       | O(n) with set         |
| Two Sum              | O(n²)       | O(n) with dict        |
| Membership filtering | O(n*m)      | O(n+m) with set       |
| Sorting              | —           | O(n log n)            |
| Binary search        | O(n) scan   | O(log n) sorted input |

---

# Final Key Takeaways

Complexity analysis is not optional.

For senior interviews, always explain:

* Time complexity
* Space complexity
* Bottleneck
* Optimization
* Tradeoff

Do not only say:

```text
This is O(n)
```

Explain why.

A strong answer sounds like:

```text
The brute-force approach is O(n²) because we compare every pair. We can optimize using a dictionary to store previous values, which gives O(n) time at the cost of O(n) extra space.
```

That is the level expected from Senior Software Engineers and AI Engineers.
