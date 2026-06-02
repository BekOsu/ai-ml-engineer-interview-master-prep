# Python Common Coding Patterns

> Reusable problem-solving patterns for Python interviews, backend engineering, data pipelines, and AI systems.

---

# Why Common Patterns Matter

Most coding interview problems are not completely new.

They usually belong to a known pattern.

If you recognize the pattern, solving becomes much easier.

Instead of thinking:

```text
I have never seen this problem before.
```

You start thinking:

```text
This is a hash map problem.
This is a sliding window problem.
This is a two-pointer problem.
This is a frequency-counting problem.
This is a grouping problem.
```

Senior engineers are expected to identify patterns quickly and explain tradeoffs clearly.

This chapter connects everything from the previous chapters:

* Lists
* Tuples
* Sets
* Dictionaries
* Functions
* Complexity analysis

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Recognize common Python coding patterns
* Choose the correct data structure
* Explain why the pattern works
* Analyze time and space complexity
* Apply patterns to interview problems
* Apply patterns to AI/backend production problems
* Move from brute force to optimized solutions

---

# Pattern 1: Frequency Counting

## When To Use

Use this pattern when the problem asks:

* Count occurrences
* Find most frequent item
* Detect duplicates
* Compare character counts
* Check anagrams
* Count labels, topics, users, events, or tokens

Common keywords:

```text
frequency
count
occurrence
repeated
most common
anagram
histogram
```

---

## Core Idea

Use a dictionary:

```python
freq = {}

for item in items:
    freq[item] = freq.get(item, 0) + 1
```

---

## Example: Character Frequency

```python
def char_frequency(text: str) -> dict[str, int]:
    freq = {}

    for char in text:
        freq[char] = freq.get(char, 0) + 1

    return freq
```

Example:

```python
print(char_frequency("banana"))
```

Output:

```python
{'b': 1, 'a': 3, 'n': 2}
```

Complexity:

```text
Time:  O(n)
Space: O(k)
```

Where `k` is the number of unique characters.

---

## AI Engineering Example

Count topics in retrieved documents:

```python
def count_topics(documents: list[dict]) -> dict[str, int]:
    counts = {}

    for doc in documents:
        topic = doc["metadata"]["topic"]
        counts[topic] = counts.get(topic, 0) + 1

    return counts
```

Useful in:

* RAG analytics
* document classification
* monitoring retrieval distribution
* dataset analysis

---

# Pattern 2: Seen Set

## When To Use

Use this pattern when the problem asks:

* Have we seen this before?
* Detect duplicates
* Avoid repeated processing
* Deduplicate results
* Prevent duplicate API/model calls

Common keywords:

```text
duplicate
unique
seen
visited
already processed
deduplicate
```

---

## Core Idea

Use a set:

```python
seen = set()

for item in items:
    if item in seen:
        ...
    seen.add(item)
```

---

## Example: Detect Duplicate

```python
def has_duplicate(nums: list[int]) -> bool:
    seen = set()

    for num in nums:
        if num in seen:
            return True

        seen.add(num)

    return False
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

## AI Engineering Example

Deduplicate chunks before embedding:

```python
def deduplicate_chunks(chunks: list[str]) -> list[str]:
    seen = set()
    unique_chunks = []

    for chunk in chunks:
        key = " ".join(chunk.strip().lower().split())

        if key in seen:
            continue

        seen.add(key)
        unique_chunks.append(chunk)

    return unique_chunks
```

Why this matters:

* reduces embedding cost
* avoids duplicate vector records
* improves retrieval quality
* improves indexing performance

---

# Pattern 3: Hash Map Lookup

## When To Use

Use this pattern when you need fast lookup by key.

Common keywords:

```text
find pair
lookup
mapping
cache
index by id
fast search
```

---

## Core Idea

Use a dictionary:

```python
lookup = {}

for item in items:
    lookup[key] = value
```

---

## Example: Two Sum

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    seen = {}

    for i, num in enumerate(nums):
        diff = target - num

        if diff in seen:
            return [seen[diff], i]

        seen[num] = i

    return []
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

## Backend Example

Build user lookup by ID:

```python
def build_user_lookup(users: list[dict]) -> dict[str, dict]:
    return {
        user["id"]: user
        for user in users
    }
```

Now lookup becomes:

```python
user = users_by_id["u123"]
```

instead of scanning a list.

---

# Pattern 4: Grouping

## When To Use

Use grouping when the problem asks:

* Group by category
* Group by key
* Group anagrams
* Group users by role
* Group documents by topic
* Group records by date

Common keywords:

```text
group
bucket
category
by type
by key
```

---

## Core Idea

Use dictionary of lists:

```python
groups = {}

for item in items:
    key = get_key(item)

    if key not in groups:
        groups[key] = []

    groups[key].append(item)
```

Or:

```python
from collections import defaultdict

groups = defaultdict(list)
```

---

## Example: Group Words By Length

```python
from collections import defaultdict

def group_by_length(words: list[str]) -> dict[int, list[str]]:
    groups = defaultdict(list)

    for word in words:
        groups[len(word)].append(word)

    return dict(groups)
```

---

## Example: Group Anagrams

```python
from collections import defaultdict

def group_anagrams(words: list[str]) -> list[list[str]]:
    groups = defaultdict(list)

    for word in words:
        key = tuple(sorted(word))
        groups[key].append(word)

    return list(groups.values())
```

Complexity:

```text
Time:  O(n * k log k)
Space: O(n * k)
```

Where:

```text
n = number of words
k = average word length
```

---

## AI Engineering Example

Group documents by source:

```python
from collections import defaultdict

def group_documents_by_source(documents: list[dict]) -> dict[str, list[dict]]:
    groups = defaultdict(list)

    for doc in documents:
        source = doc["metadata"].get("source", "unknown")
        groups[source].append(doc)

    return dict(groups)
```

Useful for:

* retrieval analysis
* source attribution
* observability
* debugging bad RAG answers

---

# Pattern 5: Two Pointers

## When To Use

Use two pointers when working with:

* sorted arrays
* pairs
* reversals
* palindromes
* merging
* in-place operations

Common keywords:

```text
sorted
pair
left and right
reverse
palindrome
merge
```

---

## Core Idea

Use two indexes moving toward each other or in the same direction.

```python
left = 0
right = len(nums) - 1

while left < right:
    ...
```

---

## Example: Reverse List In Place

```python
def reverse_list(nums: list[int]) -> list[int]:
    left = 0
    right = len(nums) - 1

    while left < right:
        nums[left], nums[right] = nums[right], nums[left]
        left += 1
        right -= 1

    return nums
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

## Example: Two Sum Sorted

```python
def two_sum_sorted(nums: list[int], target: int) -> list[int]:
    left = 0
    right = len(nums) - 1

    while left < right:
        total = nums[left] + nums[right]

        if total == target:
            return [left, right]

        if total < target:
            left += 1
        else:
            right -= 1

    return []
```

Requirement:

```text
Input must be sorted.
```

---

# Pattern 6: Sliding Window

## When To Use

Use sliding window when the problem involves:

* contiguous subarray
* substring
* max/min/sum over a range
* longest/shortest sequence
* fixed-size window
* variable-size window

Common keywords:

```text
subarray
substring
contiguous
window
longest
shortest
maximum sum
minimum length
```

---

## Fixed-Size Sliding Window

Example:

Find maximum sum of any subarray of size `k`.

```python
def max_sum_fixed_window(nums: list[int], k: int) -> int:
    if k <= 0 or k > len(nums):
        raise ValueError("Invalid window size")

    window_sum = sum(nums[:k])
    best = window_sum

    for right in range(k, len(nums)):
        window_sum += nums[right]
        window_sum -= nums[right - k]
        best = max(best, window_sum)

    return best
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

---

## Variable-Size Sliding Window

Example:

Smallest subarray length with sum >= target.

```python
def min_subarray_len(target: int, nums: list[int]) -> int:
    left = 0
    current_sum = 0
    best = float("inf")

    for right in range(len(nums)):
        current_sum += nums[right]

        while current_sum >= target:
            best = min(best, right - left + 1)
            current_sum -= nums[left]
            left += 1

    return 0 if best == float("inf") else best
```

Complexity:

```text
Time:  O(n)
Space: O(1)
```

Each pointer moves at most `n` times.

---

## AI Engineering Example

Chunk batching window:

```python
def batch_items(items: list[str], batch_size: int) -> list[list[str]]:
    batches = []

    for start in range(0, len(items), batch_size):
        batches.append(items[start:start + batch_size])

    return batches
```

Useful for:

* embedding batches
* API request batching
* inference pipelines
* document processing

---

# Pattern 7: Prefix Sum

## When To Use

Use prefix sum when you need repeated range sum queries.

Common keywords:

```text
range sum
subarray sum
sum between indexes
cumulative
```

---

## Core Idea

Precompute cumulative sums.

```python
prefix[i] = sum of elements before index i
```

---

## Example

```python
def build_prefix_sum(nums: list[int]) -> list[int]:
    prefix = [0]

    for num in nums:
        prefix.append(prefix[-1] + num)

    return prefix
```

Range sum from `left` to `right` inclusive:

```python
def range_sum(prefix: list[int], left: int, right: int) -> int:
    return prefix[right + 1] - prefix[left]
```

Example:

```python
nums = [2, 4, 6, 8]
prefix = build_prefix_sum(nums)

print(range_sum(prefix, 1, 2))
```

Output:

```python
10
```

Because:

```text
4 + 6 = 10
```

---

# Pattern 8: Sorting Then Scanning

## When To Use

Use when sorting reveals structure.

Common keywords:

```text
merge intervals
overlap
closest pair
duplicates
ranking
top
```

---

## Example: Merge Intervals

```python
def merge_intervals(intervals: list[list[int]]) -> list[list[int]]:
    if not intervals:
        return []

    intervals.sort(key=lambda interval: interval[0])

    merged = [intervals[0]]

    for start, end in intervals[1:]:
        last = merged[-1]

        if start <= last[1]:
            last[1] = max(last[1], end)
        else:
            merged.append([start, end])

    return merged
```

Complexity:

```text
Time:  O(n log n)
Space: O(n)
```

Sorting dominates.

---

## Backend Example

Merge overlapping booking windows, API rate-limit windows, or scheduled jobs.

---

# Pattern 9: Stack

## When To Use

Use stack when the problem involves:

* last-in-first-out behavior
* matching parentheses
* undo operations
* nested structures
* parsing
* monotonic stack problems

Common keywords:

```text
valid parentheses
nested
last opened
undo
previous greater
next greater
```

---

## Core Idea

Use a list as a stack:

```python
stack = []

stack.append(x)
stack.pop()
```

---

## Example: Valid Parentheses

```python
def is_valid_parentheses(text: str) -> bool:
    stack = []
    pairs = {
        ")": "(",
        "]": "[",
        "}": "{"
    }

    for char in text:
        if char in "([{":
            stack.append(char)
        elif char in pairs:
            if not stack or stack[-1] != pairs[char]:
                return False

            stack.pop()

    return len(stack) == 0
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# Pattern 10: Queue

## When To Use

Use queues when processing items in first-in-first-out order.

Common keywords:

```text
BFS
level order
task queue
producer consumer
first come first served
```

Use `collections.deque`.

---

## Core Idea

```python
from collections import deque

queue = deque()
queue.append(item)
queue.popleft()
```

Avoid:

```python
list.pop(0)
```

because it is O(n).

---

## Example: Process Tasks

```python
from collections import deque

def process_tasks(tasks: list[str]) -> list[str]:
    queue = deque(tasks)
    processed = []

    while queue:
        task = queue.popleft()
        processed.append(task)

    return processed
```

Complexity:

```text
Time:  O(n)
Space: O(n)
```

---

# Pattern 11: Heap / Priority Queue

## When To Use

Use heap when you need:

* top k
* smallest item
* largest item
* priority-based processing
* scheduling

Common keywords:

```text
top k
k largest
k smallest
priority
minimum
maximum
```

---

## Core Idea

Use `heapq`.

```python
import heapq

heap = []
heapq.heappush(heap, value)
smallest = heapq.heappop(heap)
```

---

## Example: Top K Largest

```python
import heapq

def top_k_largest(nums: list[int], k: int) -> list[int]:
    return heapq.nlargest(k, nums)
```

Complexity:

```text
Time:  O(n log k)
Space: O(k)
```

---

## AI Engineering Example

Return top-k retrieval results:

```python
import heapq

def top_k_results(results: list[dict], k: int) -> list[dict]:
    return heapq.nlargest(
        k,
        results,
        key=lambda item: item["score"]
    )
```

Useful in:

* vector search
* reranking
* recommendation systems
* search pipelines

---

# Pattern 12: Normalize Then Compare

## When To Use

Use when raw values may differ but should be treated as equivalent.

Common cases:

* text comparison
* email comparison
* deduplication
* search indexing
* cache keys

---

## Example

```python
def normalize_text(text: str) -> str:
    return " ".join(
        text.strip().lower().split()
    )
```

Use:

```python
def are_same_text(a: str, b: str) -> bool:
    return normalize_text(a) == normalize_text(b)
```

---

## AI Engineering Example

Before embedding cache lookup:

```python
def build_text_key(text: str) -> str:
    return normalize_text(text)
```

This avoids treating these as different:

```text
"Python Functions"
" python   functions "
"PYTHON FUNCTIONS"
```

---

# Pattern 13: Validate Early

## When To Use

Use this pattern in production functions.

Check invalid input at the start.

---

## Example

```python
def average(nums: list[float]) -> float:
    if not nums:
        raise ValueError("nums must not be empty")

    return sum(nums) / len(nums)
```

Benefits:

* clearer error messages
* safer code
* easier debugging
* fewer hidden failures

---

# Pattern 14: Transform Pipeline

## When To Use

Use when data passes through stages.

Common in:

* ETL
* feature engineering
* RAG
* data cleaning
* API request processing

---

## Example

```python
def clean_text(text: str) -> str:
    return " ".join(text.strip().split())

def lowercase(text: str) -> str:
    return text.lower()

def remove_empty(items: list[str]) -> list[str]:
    return [
        item
        for item in items
        if item
    ]

def process_texts(texts: list[str]) -> list[str]:
    cleaned = [clean_text(text) for text in texts]
    lowered = [lowercase(text) for text in cleaned]
    return remove_empty(lowered)
```

---

## AI Engineering Example

RAG preprocessing:

```python
def preprocess_documents(documents: list[str]) -> list[str]:
    cleaned = [normalize_text(doc) for doc in documents]
    unique = deduplicate_chunks(cleaned)
    return unique
```

---

# Pattern 15: Brute Force Then Optimize

## When To Use

Use in interviews.

Do not freeze trying to find the perfect answer immediately.

Start with a correct brute-force solution.

Then optimize.

---

## Interview Structure

```text
1. Clarify problem
2. State brute-force approach
3. Analyze brute force
4. Identify bottleneck
5. Optimize with better data structure
6. Analyze optimized solution
7. Test with examples
```

---

## Example Explanation

```text
The brute-force solution checks every pair, so it is O(n²). We can optimize by storing previously seen values in a dictionary. That gives O(n) time but uses O(n) extra space.
```

This is exactly how strong candidates communicate.

---

# Pattern Selection Guide

| Problem Type         | Likely Pattern          |
| -------------------- | ----------------------- |
| Count occurrences    | Frequency dictionary    |
| Detect duplicates    | Seen set                |
| Find pair            | Hash map / two pointers |
| Sorted array pair    | Two pointers            |
| Contiguous subarray  | Sliding window          |
| Range sums           | Prefix sum              |
| Group items          | Dictionary of lists     |
| Top K                | Heap                    |
| Matching parentheses | Stack                   |
| FIFO processing      | Queue                   |
| Deduplicate text     | Normalize + set         |
| Pipeline processing  | Transform pipeline      |

---

# Production Engineering Notes

Common patterns are not only for LeetCode.

They appear in real systems.

## Backend

* permission checks → set
* routing handlers → dictionary
* API validation → validate early
* task queues → queue
* caching → dictionary / Redis

## Data Engineering

* group records by date → dictionary of lists
* count event types → frequency counter
* remove duplicate records → seen set
* process batches → sliding window / batching

## AI Engineering

* deduplicate chunks → seen set
* cache embeddings → dictionary
* top-k results → heap
* chunk batching → batching window
* normalize text → normalize then compare
* RAG pipeline → transform pipeline

---

# Interview Questions And Answers

## Q1. What is a coding pattern?

A coding pattern is a reusable problem-solving approach that applies to a family of problems.

For example, frequency counting uses a dictionary to count occurrences.

---

## Q2. How do you recognize a frequency-counting problem?

Look for words like:

* count
* occurrence
* repeated
* most frequent
* anagram

These usually suggest a dictionary or `Counter`.

---

## Q3. How do you recognize a sliding window problem?

Look for:

* contiguous subarray
* substring
* longest
* shortest
* fixed-size range
* moving window

---

## Q4. When do you use a set?

Use a set when you need:

* fast membership checks
* uniqueness
* deduplication
* seen/visited tracking

---

## Q5. When do you use a dictionary?

Use a dictionary when you need:

* key-value lookup
* frequency counting
* grouping
* caching
* indexing records by ID

---

## Q6. When do you use two pointers?

Use two pointers when:

* the input is sorted
* you need to find pairs
* you need to reverse in place
* you need to scan from both ends

---

## Q7. Why is brute force still useful?

Brute force helps you understand the problem and produce a correct baseline.

Then you can optimize the bottleneck.

Interviewers like seeing this progression.

---

## Q8. How do common patterns appear in AI systems?

Examples:

* seen set for deduplicating chunks
* dictionary for embedding cache
* heap for top-k retrieval
* pipeline functions for RAG processing
* normalization for cache keys and deduplication

---

# Senior-Level Questions And Answers

## Senior Q1. How do you choose between list, set, and dict?

Use list when order and iteration matter.

Use set when uniqueness and membership checks matter.

Use dict when mapping keys to values matters.

The choice should be based on required operations and complexity.

---

## Senior Q2. How would you optimize repeated membership checks?

If membership is checked repeatedly against a list, convert the list to a set once.

```python
allowed = set(allowed_ids)
```

This changes repeated lookup from O(n) to O(1) average.

---

## Senior Q3. How would you design a RAG preprocessing pipeline?

Break it into stages:

```python
def normalize_documents(docs):
    ...

def deduplicate_documents(docs):
    ...

def chunk_documents(docs):
    ...

def batch_chunks(chunks):
    ...
```

Each stage should be small, testable, and observable.

---

## Senior Q4. How do you avoid overengineering patterns?

Use the simplest pattern that solves the problem clearly.

Do not use a heap if sorting is simpler and input size is small.

Do not use complex abstractions before measuring bottlenecks.

Senior engineers balance simplicity, correctness, and scalability.

---

## Senior Q5. How do you explain optimization in an interview?

Use this structure:

```text
The brute-force solution is correct but slow because...
The bottleneck is...
We can improve it using...
The optimized complexity is...
The tradeoff is...
```

This shows senior-level reasoning.

---

# Exercises

## Exercise 1 — Frequency Count

Write:

```python
def count_items(items):
    pass
```

Return a dictionary of item counts.

---

## Exercise 2 — First Duplicate

Write:

```python
def first_duplicate(items):
    pass
```

Return the first item that appears twice.

---

## Exercise 3 — Group By First Letter

Given a list of words, group them by first letter.

---

## Exercise 4 — Two Sum

Solve Two Sum using a dictionary.

---

## Exercise 5 — Reverse With Two Pointers

Reverse a list in place using two pointers.

---

## Exercise 6 — Max Sum Fixed Window

Find maximum sum of a subarray of size `k`.

---

## Exercise 7 — Prefix Sum Range Query

Build prefix sums and answer range sum queries.

---

## Exercise 8 — Merge Intervals

Merge overlapping intervals.

---

## Exercise 9 — Valid Parentheses

Check if parentheses are valid.

---

## Exercise 10 — Top K Scores

Given retrieval results with scores, return top `k`.

---

## Exercise 11 — Deduplicate RAG Chunks

Normalize and deduplicate text chunks.

---

## Exercise 12 — Build RAG Mini Pipeline

Create a simple pipeline:

```text
normalize
deduplicate
batch
```

---

# Solutions

## Solution 1

```python
def count_items(items):
    counts = {}

    for item in items:
        counts[item] = counts.get(item, 0) + 1

    return counts
```

---

## Solution 2

```python
def first_duplicate(items):
    seen = set()

    for item in items:
        if item in seen:
            return item

        seen.add(item)

    return None
```

---

## Solution 3

```python
from collections import defaultdict

def group_by_first_letter(words):
    groups = defaultdict(list)

    for word in words:
        if not word:
            continue

        groups[word[0]].append(word)

    return dict(groups)
```

---

## Solution 4

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

---

## Solution 5

```python
def reverse_in_place(nums):
    left = 0
    right = len(nums) - 1

    while left < right:
        nums[left], nums[right] = nums[right], nums[left]
        left += 1
        right -= 1

    return nums
```

---

## Solution 6

```python
def max_sum_fixed_window(nums, k):
    if k <= 0 or k > len(nums):
        raise ValueError("Invalid k")

    window_sum = sum(nums[:k])
    best = window_sum

    for right in range(k, len(nums)):
        window_sum += nums[right]
        window_sum -= nums[right - k]
        best = max(best, window_sum)

    return best
```

---

## Solution 7

```python
def build_prefix_sum(nums):
    prefix = [0]

    for num in nums:
        prefix.append(prefix[-1] + num)

    return prefix

def range_sum(prefix, left, right):
    return prefix[right + 1] - prefix[left]
```

---

## Solution 8

```python
def merge_intervals(intervals):
    if not intervals:
        return []

    intervals.sort(key=lambda interval: interval[0])
    merged = [intervals[0]]

    for start, end in intervals[1:]:
        last = merged[-1]

        if start <= last[1]:
            last[1] = max(last[1], end)
        else:
            merged.append([start, end])

    return merged
```

---

## Solution 9

```python
def is_valid_parentheses(text):
    stack = []
    pairs = {
        ")": "(",
        "]": "[",
        "}": "{"
    }

    for char in text:
        if char in "([{":
            stack.append(char)
        elif char in pairs:
            if not stack or stack[-1] != pairs[char]:
                return False

            stack.pop()

    return len(stack) == 0
```

---

## Solution 10

```python
import heapq

def top_k_scores(results, k):
    return heapq.nlargest(
        k,
        results,
        key=lambda item: item["score"]
    )
```

---

## Solution 11

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

## Solution 12

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

def batch_items(items, batch_size):
    batches = []

    for start in range(0, len(items), batch_size):
        batches.append(items[start:start + batch_size])

    return batches

def rag_preprocess_pipeline(chunks, batch_size):
    normalized = [normalize_text(chunk) for chunk in chunks]
    unique = deduplicate_chunks(normalized)
    return batch_items(unique, batch_size)
```

---

# Mock Interview

## Interviewer

How do you approach an unfamiliar coding problem?

## Strong Candidate Answer

I first clarify the input and output, then propose a brute-force solution. After that, I analyze the complexity, identify the bottleneck, and optimize using a suitable pattern like a hash map, set, two pointers, or sliding window.

---

## Interviewer

How do you know when to use a dictionary?

## Strong Candidate Answer

I use a dictionary when I need fast key-value lookup, frequency counting, grouping, caching, or indexing records by ID.

---

## Interviewer

How do you know when to use a sliding window?

## Strong Candidate Answer

I look for contiguous subarray or substring problems, especially when the question asks for longest, shortest, max, min, or sum over a range.

---

## Interviewer

How would you deduplicate chunks before embedding?

## Strong Candidate Answer

I would normalize each chunk, store normalized versions in a set, and keep only the first unseen chunk. This gives O(n) time and prevents duplicate embedding calls.

---

## Interviewer

How would you get top 5 retrieval results?

## Strong Candidate Answer

If I already have all scores, I can sort by score descending. For large data or streaming cases, I can use a heap to maintain top-k efficiently.

---

# Revision Sheet

## Pattern Map

| Need                   | Pattern                |
| ---------------------- | ---------------------- |
| Count                  | Frequency dict         |
| Seen before            | Set                    |
| Fast lookup            | Dict                   |
| Grouping               | Dict of lists          |
| Sorted pair            | Two pointers           |
| Contiguous range       | Sliding window         |
| Range sums             | Prefix sum             |
| Overlaps               | Sort + scan            |
| Nested matching        | Stack                  |
| FIFO processing        | Queue                  |
| Top K                  | Heap                   |
| Text equality          | Normalize then compare |
| Safe input             | Validate early         |
| Multi-stage processing | Transform pipeline     |

---

# Cheat Sheet

## Frequency

```python
freq[x] = freq.get(x, 0) + 1
```

## Seen Set

```python
if x in seen:
    ...
seen.add(x)
```

## Grouping

```python
groups.setdefault(key, []).append(value)
```

## Two Pointers

```python
left = 0
right = len(nums) - 1
```

## Sliding Window

```python
window_sum += nums[right]
window_sum -= nums[left]
```

## Prefix Sum

```python
prefix[right + 1] - prefix[left]
```

## Stack

```python
stack.append(x)
stack.pop()
```

## Queue

```python
from collections import deque
queue.popleft()
```

## Heap

```python
heapq.nlargest(k, items)
```

---

# Final Key Takeaways

Coding interviews become easier when you recognize patterns.

Do not memorize only solutions.

Learn to recognize:

* what the problem is asking
* which data structure fits
* which complexity is acceptable
* what tradeoff you are making

For Senior Software Engineer and AI Engineer roles, these same patterns appear in production:

* deduplication
* caching
* retrieval
* ranking
* filtering
* batching
* preprocessing
* pipeline design

Master these patterns before moving to debugging and performance.
