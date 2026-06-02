# Core Python Solutions 26–50

> Detailed solutions for Core Python Exercises 26–50.

---

# How To Use This File

Do not only read the final code.

For each solution, study:

1. The approach
2. The data structure choice
3. Time complexity
4. Space complexity
5. Edge cases
6. Production relevance

These exercises connect Core Python fundamentals with real backend and AI engineering patterns.

---

# Exercise 26 — Move Zeros To End

## Solution: New List Approach

```python
def move_zeros(nums: list[int]) -> list[int]:
    non_zeros = []
    zero_count = 0

    for num in nums:
        if num == 0:
            zero_count += 1
        else:
            non_zeros.append(num)

    return non_zeros + [0] * zero_count
```

## Explanation

We keep all non-zero values in order, count zeros, then append zeros at the end.

Example:

```python
move_zeros([0, 1, 0, 3, 12])
```

Output:

```python
[1, 3, 12, 0, 0]
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

## In-Place Alternative

```python
def move_zeros_in_place(nums: list[int]) -> list[int]:
    write_index = 0

    for read_index in range(len(nums)):
        if nums[read_index] != 0:
            nums[write_index] = nums[read_index]
            write_index += 1

    for i in range(write_index, len(nums)):
        nums[i] = 0

    return nums
```

## Complexity

```text
Time:  O(n)
Space: O(1)
```

## Interview Note

The in-place version is stronger for interviews because it avoids extra memory.

---

# Exercise 27 — Find Second Largest

## Solution

```python
def second_largest(nums: list[int]) -> int:
    if len(nums) < 2:
        raise ValueError("At least two numbers are required")

    largest = None
    second = None

    for num in nums:
        if largest is None or num > largest:
            second = largest
            largest = num
        elif num != largest and (second is None or num > second):
            second = num

    if second is None:
        raise ValueError("Second largest value does not exist")

    return second
```

## Explanation

We track two values:

```text
largest
second largest unique value
```

Duplicates of the largest value do not count as second largest.

Example:

```python
second_largest([5, 1, 7, 7, 3])
```

Output:

```python
5
```

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 28 — Is Palindrome List

## Solution

```python
def is_palindrome(items: list[int]) -> bool:
    left = 0
    right = len(items) - 1

    while left < right:
        if items[left] != items[right]:
            return False

        left += 1
        right -= 1

    return True
```

## Explanation

We compare values from both ends and move inward.

Example:

```python
[1, 2, 3, 2, 1]
```

Comparisons:

```text
1 == 1
2 == 2
middle reached
```

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 29 — Flatten Nested List One Level

## Solution

```python
def flatten_once(matrix: list[list[int]]) -> list[int]:
    result = []

    for row in matrix:
        for item in row:
            result.append(item)

    return result
```

## List Comprehension Version

```python
def flatten_once(matrix: list[list[int]]) -> list[int]:
    return [
        item
        for row in matrix
        for item in row
    ]
```

## Explanation

This flattens only one level.

Input:

```python
[[1, 2], [3, 4], [5]]
```

Output:

```python
[1, 2, 3, 4, 5]
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

Where `n` is the total number of inner items.

---

# Exercise 30 — Chunk List Into Batches

## Solution

```python
def chunk_list(items: list, batch_size: int) -> list[list]:
    if batch_size <= 0:
        raise ValueError("batch_size must be positive")

    batches = []

    for start in range(0, len(items), batch_size):
        batches.append(items[start:start + batch_size])

    return batches
```

## Explanation

We move through the list in steps of `batch_size`.

Example:

```python
chunk_list([1, 2, 3, 4, 5], 2)
```

Output:

```python
[[1, 2], [3, 4], [5]]
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

## AI Engineering Note

This pattern is used for:

* embedding batches
* API batches
* inference batches
* database inserts
* file processing

---

# Exercise 31 — First Repeated Character

## Solution

```python
def first_repeated_char(text: str) -> str | None:
    seen = set()

    for char in text:
        if char in seen:
            return char

        seen.add(char)

    return None
```

## Explanation

We scan the string from left to right.

The first character already in `seen` is the first repeated character.

Example:

```python
first_repeated_char("abca")
```

Output:

```python
"a"
```

## Complexity

```text
Time:  O(n)
Space: O(k)
```

Where `k` is the number of unique characters.

---

# Exercise 32 — Are Anagrams

## Solution: Frequency Dictionary

```python
def normalize(text: str) -> str:
    return text.replace(" ", "").lower()


def char_count(text: str) -> dict[str, int]:
    counts = {}

    for char in text:
        counts[char] = counts.get(char, 0) + 1

    return counts


def are_anagrams(a: str, b: str) -> bool:
    a = normalize(a)
    b = normalize(b)

    if len(a) != len(b):
        return False

    return char_count(a) == char_count(b)
```

## Explanation

Two strings are anagrams if they contain the same characters with the same frequencies.

Example:

```python
are_anagrams("listen", "silent")
```

Output:

```python
True
```

## Complexity

```text
Time:  O(n)
Space: O(k)
```

---

## Sorting Alternative

```python
def are_anagrams(a: str, b: str) -> bool:
    a = a.replace(" ", "").lower()
    b = b.replace(" ", "").lower()

    return sorted(a) == sorted(b)
```

## Complexity

```text
Time:  O(n log n)
Space: O(n)
```

## Interview Note

The frequency dictionary version is more optimal.

The sorting version is shorter.

---

# Exercise 33 — Most Frequent Item

## Solution

```python
def most_frequent(items: list[str]) -> str:
    if not items:
        raise ValueError("items must not be empty")

    counts = {}

    for item in items:
        counts[item] = counts.get(item, 0) + 1

    best_item = None
    best_count = 0

    for item, count in counts.items():
        if count > best_count:
            best_item = item
            best_count = count

    return best_item
```

## Explanation

We count frequencies, then find the item with the highest count.

Example:

```python
most_frequent(["ai", "python", "ai", "rag", "ai"])
```

Output:

```python
"ai"
```

## Complexity

```text
Time:  O(n)
Space: O(k)
```

Where `k` is the number of unique items.

---

# Exercise 34 — Invert Dictionary

## Solution

```python
def invert_dict(data: dict[str, str]) -> dict[str, list[str]]:
    inverted = {}

    for key, value in data.items():
        if value not in inverted:
            inverted[value] = []

        inverted[value].append(key)

    return inverted
```

## Explanation

Input:

```python
{
    "u1": "admin",
    "u2": "viewer",
    "u3": "admin",
}
```

Output:

```python
{
    "admin": ["u1", "u3"],
    "viewer": ["u2"]
}
```

Because multiple users can have the same role, the inverted dictionary maps each role to a list of users.

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# Exercise 35 — Build Lookup By ID

## Solution

```python
def build_lookup_by_id(records: list[dict]) -> dict:
    lookup = {}

    for record in records:
        if "id" not in record:
            raise ValueError(f"Record missing id: {record}")

        lookup[record["id"]] = record

    return lookup
```

## Explanation

This turns a list of records into a dictionary indexed by ID.

Before:

```python
users = [
    {"id": "u1", "name": "Ali"},
    {"id": "u2", "name": "Sara"},
]
```

Lookup requires scanning:

```text
O(n)
```

After:

```python
users_by_id = {
    "u1": {"id": "u1", "name": "Ali"},
    "u2": {"id": "u2", "name": "Sara"},
}
```

Lookup:

```python
users_by_id["u1"]
```

Average:

```text
O(1)
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# Exercise 36 — Validate User Payload

## Solution

```python
def validate_user_payload(payload: dict) -> bool:
    required_fields = ["name", "email", "role"]

    for field in required_fields:
        if field not in payload:
            return False

        if not payload[field]:
            return False

    email = payload["email"]

    if "@" not in email:
        return False

    return True
```

## Explanation

We check:

* required fields exist
* required fields are not empty
* email contains `@`

## Complexity

```text
Time:  O(1)
Space: O(1)
```

Because the number of required fields is fixed.

## Production Note

In real FastAPI applications, use Pydantic models for validation.

---

# Exercise 37 — Safe Get Nested Value

## Solution

```python
def get_nested(data: dict, keys: list[str], default=None):
    current = data

    for key in keys:
        if not isinstance(current, dict):
            return default

        if key not in current:
            return default

        current = current[key]

    return current
```

## Explanation

We walk through the dictionary one key at a time.

Example:

```python
data = {
    "metadata": {
        "source": "core-python"
    }
}

get_nested(data, ["metadata", "source"])
```

Output:

```python
"core-python"
```

## Complexity

```text
Time:  O(k)
Space: O(1)
```

Where `k` is the number of nested keys.

---

# Exercise 38 — Retry Function

## Solution

```python
def retry(func, attempts: int = 3):
    if attempts <= 0:
        raise ValueError("attempts must be positive")

    last_error = None

    for _ in range(attempts):
        try:
            return func()
        except Exception as error:
            last_error = error

    raise last_error
```

## Explanation

We call the function up to `attempts` times.

If all attempts fail, we raise the last exception.

## Production Improvements

A production retry function may include:

* delay
* exponential backoff
* jitter
* logging
* retry only specific exceptions
* timeout
* circuit breaker

## Complexity

```text
Time:  O(attempts * cost(func))
Space: O(1)
```

---

# Exercise 39 — Apply Transform Pipeline

## Solution

```python
def apply_pipeline(value, functions: list):
    result = value

    for func in functions:
        result = func(result)

    return result
```

## Explanation

Each function receives the output of the previous function.

Example:

```python
def strip_text(text):
    return text.strip()

def lower_text(text):
    return text.lower()

apply_pipeline("  HELLO  ", [strip_text, lower_text])
```

Output:

```python
"hello"
```

## Complexity

```text
Time:  O(number_of_functions * average_function_cost)
Space: depends on transformations
```

---

# Exercise 40 — Create Prefix Function

## Solution

```python
def make_prefixer(prefix: str):
    def add_prefix(text: str) -> str:
        return f"{prefix}{text}"

    return add_prefix
```

## Usage

```python
error = make_prefixer("ERROR: ")

print(error("Invalid input"))
```

Output:

```python
ERROR: Invalid input
```

## Explanation

This is a closure.

The inner function remembers `prefix` from the outer function even after `make_prefixer()` finishes.

## Complexity

```text
Time:  O(len(prefix) + len(text))
Space: O(len(prefix) + len(text))
```

---

# Exercise 41 — Fix Empty Max Bug

## Solution

```python
def broken_max(nums):
    if not nums:
        raise ValueError("nums must not be empty")

    maximum = nums[0]

    for num in nums:
        if num > maximum:
            maximum = num

    return maximum
```

## Explanation

The original function failed on empty input because:

```python
nums[0]
```

raises `IndexError` when the list is empty.

We fail early with a clear `ValueError`.

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 42 — Fix Shared Nested List Bug

## Solution

```python
matrix = [
    [0] * 3
    for _ in range(3)
]
```

## Explanation

Bad version:

```python
matrix = [[0] * 3] * 3
```

creates three references to the same inner list.

Correct version creates a new list for each row.

## Test

```python
matrix[0][0] = 1

print(matrix)
```

Output:

```python
[
    [1, 0, 0],
    [0, 0, 0],
    [0, 0, 0],
]
```

## Complexity

```text
Time:  O(rows * cols)
Space: O(rows * cols)
```

---

# Exercise 43 — Remove None Values

## Solution

```python
def remove_none_values(data: dict) -> dict:
    return {
        key: value
        for key, value in data.items()
        if value is not None
    }
```

## Explanation

We create a new dictionary instead of deleting keys while iterating.

This avoids:

```text
RuntimeError: dictionary changed size during iteration
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# Exercise 44 — Safe Normalize Text

## Solution

```python
def safe_normalize_text(text: str | None) -> str:
    if text is None:
        return ""

    return " ".join(
        text.strip().lower().split()
    )
```

## Explanation

This handles `None` safely.

It also normalizes:

```text
"  Python   FUNCTIONS "
```

into:

```text
"python functions"
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

Where `n` is text length.

---

# Exercise 45 — Debug Wrong Initial Value

## Solution

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

## Explanation

The bug was initializing:

```python
best = 0
```

This fails for all-negative lists.

Example:

```python
[-5, -2, -9]
```

Correct result:

```python
-2
```

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 46 — Optimize Permission Filtering

## Solution

```python
def filter_allowed_documents(documents, allowed_ids):
    allowed = set(allowed_ids)

    result = []

    for doc in documents:
        if doc["id"] in allowed:
            result.append(doc)

    return result
```

## Explanation

Original version:

```python
if doc["id"] in allowed_ids:
```

If `allowed_ids` is a list, each lookup is O(m).

For `n` documents:

```text
O(n * m)
```

Optimized version converts `allowed_ids` to a set.

Set membership is O(1) average.

## Complexity

```text
Time:  O(n + m)
Space: O(m)
```

Where:

```text
n = number of documents
m = number of allowed IDs
```

---

# Exercise 47 — Deduplicate Search Results

## Solution

```python
def deduplicate_results(results: list[dict]) -> list[dict]:
    seen = set()
    final = []

    for result in results:
        document_id = result["document_id"]

        if document_id in seen:
            continue

        seen.add(document_id)
        final.append(result)

    return final
```

## Explanation

We preserve the first occurrence of each document.

This is useful when multiple retrievers return overlapping documents.

Example:

```python
[
    {"document_id": "d1", "score": 0.9},
    {"document_id": "d2", "score": 0.8},
    {"document_id": "d1", "score": 0.7},
]
```

Output:

```python
[
    {"document_id": "d1", "score": 0.9},
    {"document_id": "d2", "score": 0.8},
]
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# Exercise 48 — Top K Search Results

## Solution: Sorting

```python
def top_k_results(results: list[dict], k: int) -> list[dict]:
    if k <= 0:
        return []

    return sorted(
        results,
        key=lambda item: item["score"],
        reverse=True
    )[:k]
```

## Explanation

We sort by score descending and take the first `k`.

## Complexity

```text
Time:  O(n log n)
Space: O(n)
```

---

## Heap Alternative

```python
import heapq

def top_k_results_heap(results: list[dict], k: int) -> list[dict]:
    if k <= 0:
        return []

    return heapq.nlargest(
        k,
        results,
        key=lambda item: item["score"]
    )
```

## Heap Complexity

```text
Time:  O(n log k)
Space: O(k)
```

## Interview Note

Sorting is simpler.

Heap is better when:

```text
n is large
k is small
```

---

# Exercise 49 — Batch Embedding Requests

## Solution

```python
def create_embedding_batches(
    texts: list[str],
    batch_size: int
) -> list[list[str]]:
    if batch_size <= 0:
        raise ValueError("batch_size must be positive")

    batches = []

    for start in range(0, len(texts), batch_size):
        batches.append(texts[start:start + batch_size])

    return batches
```

## Explanation

We preserve order and split texts into chunks.

Example:

```python
create_embedding_batches(["a", "b", "c", "d", "e"], 2)
```

Output:

```python
[
    ["a", "b"],
    ["c", "d"],
    ["e"],
]
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

## AI Engineering Note

Batching improves throughput and reduces API overhead.

---

# Exercise 50 — Mini RAG Preprocessing Pipeline

## Solution

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

## Example

```python
rag_preprocess([
    " Python   Functions ",
    "python functions",
    "",
    " Dictionaries ",
    "DICTIONARIES"
], batch_size=2)
```

Output:

```python
[
    ["python functions", "dictionaries"]
]
```

## Explanation

Pipeline stages:

```text
normalize
↓
remove empty chunks
↓
deduplicate
↓
batch
```

Why normalize first?

Because these should be treated as duplicates:

```text
" Python   Functions "
"python functions"
"PYTHON FUNCTIONS"
```

Why batch?

Because embedding APIs and model inference usually work better in batches.

## Complexity

Let:

```text
n = number of chunks
k = average chunk length
```

Then:

```text
Time:  O(n * k)
Space: O(n * k)
```

---

# Final Review

Exercises 26–50 trained:

* two-pointer patterns
* list batching
* set-based deduplication
* dictionary lookups
* closures
* retry logic
* safe validation
* debugging
* performance optimization
* RAG preprocessing

These are practical skills for:

* Python interviews
* backend services
* FastAPI
* ML pipelines
* RAG systems
* AI platform engineering

---

# Mastery Checklist

You are ready for the interview questions file if you can explain:

* [ ] Why `set` improves repeated lookup
* [ ] Why wrong initial values break negative-number logic
* [ ] Why modifying dictionaries during iteration is dangerous
* [ ] Why RAG chunk deduplication needs normalization
* [ ] Why embedding requests should be batched
* [ ] Why cache keys need model/version/text hash
* [ ] Why heap can be better than sorting for top-k
* [ ] Why closures are useful
* [ ] Why retry functions should not swallow errors
* [ ] Why preprocessing pipelines should be split into stages

---

# Final Note

The final exercise, Mini RAG Preprocessing Pipeline, is especially important.

It combines many Core Python concepts:

```text
functions
lists
sets
dictionaries
normalization
deduplication
batching
validation
complexity analysis
```

This is exactly how Core Python becomes useful in real AI engineering work.
