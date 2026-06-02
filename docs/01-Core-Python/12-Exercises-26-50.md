# Core Python Exercises 26–50

> Second practice set for Core Python: advanced list patterns, dictionaries, functions, debugging, performance, and AI engineering use cases.

---

# How To Use This File

This file continues from `11-Exercises-01-25.md`.

Do not open the solutions file first.

For every exercise:

1. Write the solution yourself.
2. Test with the example.
3. Add at least two edge cases.
4. Write time complexity.
5. Write space complexity.
6. Explain your approach out loud as if you are in an interview.

The goal is to build interview confidence, not only solve the problem.

---

# Section 1 — Lists And Common Patterns

---

## Exercise 26 — Move Zeros To End

**Difficulty:** Medium

Move all zeros to the end of the list while keeping the relative order of non-zero elements.

```python
def move_zeros(nums: list[int]) -> list[int]:
    pass
```

### Example

```python
move_zeros([0, 1, 0, 3, 12])
```

Expected output:

```python
[1, 3, 12, 0, 0]
```

### Requirements

* Preserve order of non-zero elements.
* Return a new list or modify in place.
* Explain your choice.

### Think About

* Can you do it in O(n)?
* Can you do it with O(1) extra space?

---

## Exercise 27 — Find Second Largest

**Difficulty:** Medium

Find the second largest unique number in a list.

```python
def second_largest(nums: list[int]) -> int:
    pass
```

### Example

```python
second_largest([5, 1, 7, 7, 3])
```

Expected output:

```python
5
```

### Requirements

* Do not sort the list.
* Handle duplicates.
* If second largest does not exist, raise `ValueError`.

---

## Exercise 28 — Is Palindrome List

**Difficulty:** Easy

Check if a list reads the same forward and backward.

```python
def is_palindrome(items: list[int]) -> bool:
    pass
```

### Example

```python
is_palindrome([1, 2, 3, 2, 1])
```

Expected output:

```python
True
```

### Requirements

* Use the two-pointer pattern.
* Do not use slicing.

---

## Exercise 29 — Flatten Nested List One Level

**Difficulty:** Medium

Flatten a list of lists one level.

```python
def flatten_once(matrix: list[list[int]]) -> list[int]:
    pass
```

### Example

```python
flatten_once([[1, 2], [3, 4], [5]])
```

Expected output:

```python
[1, 2, 3, 4, 5]
```

### Requirements

* Do not use external libraries.
* Use a clear loop or list comprehension.

---

## Exercise 30 — Chunk List Into Batches

**Difficulty:** Medium

Split a list into batches of size `batch_size`.

```python
def chunk_list(items: list, batch_size: int) -> list[list]:
    pass
```

### Example

```python
chunk_list([1, 2, 3, 4, 5], 2)
```

Expected output:

```python
[[1, 2], [3, 4], [5]]
```

### Requirements

* If `batch_size <= 0`, raise `ValueError`.
* This pattern is useful for embedding batches and API calls.

---

# Section 2 — Sets And Dictionaries

---

## Exercise 31 — First Repeated Character

**Difficulty:** Easy

Return the first character that appears more than once.

```python
def first_repeated_char(text: str) -> str | None:
    pass
```

### Example

```python
first_repeated_char("abca")
```

Expected output:

```python
"a"
```

### Requirements

* Use a set.
* Return `None` if no repeated character exists.

---

## Exercise 32 — Are Anagrams

**Difficulty:** Medium

Check if two strings are anagrams.

```python
def are_anagrams(a: str, b: str) -> bool:
    pass
```

### Example

```python
are_anagrams("listen", "silent")
```

Expected output:

```python
True
```

### Requirements

* Ignore case.
* Ignore spaces.
* Use either sorting or frequency dictionaries.
* Explain complexity of your chosen approach.

---

## Exercise 33 — Most Frequent Item

**Difficulty:** Medium

Return the most frequent item in a list.

```python
def most_frequent(items: list[str]) -> str:
    pass
```

### Example

```python
most_frequent(["ai", "python", "ai", "rag", "ai"])
```

Expected output:

```python
"ai"
```

### Requirements

* If list is empty, raise `ValueError`.
* Use a dictionary.

---

## Exercise 34 — Invert Dictionary

**Difficulty:** Medium

Invert a dictionary where values may repeat.

```python
def invert_dict(data: dict[str, str]) -> dict[str, list[str]]:
    pass
```

### Example

```python
invert_dict({
    "u1": "admin",
    "u2": "viewer",
    "u3": "admin",
})
```

Expected output:

```python
{
    "admin": ["u1", "u3"],
    "viewer": ["u2"]
}
```

---

## Exercise 35 — Build Lookup By ID

**Difficulty:** Medium

Given a list of dictionaries, build a dictionary indexed by `id`.

```python
def build_lookup_by_id(records: list[dict]) -> dict:
    pass
```

### Example

```python
build_lookup_by_id([
    {"id": "u1", "name": "Ali"},
    {"id": "u2", "name": "Sara"},
])
```

Expected output:

```python
{
    "u1": {"id": "u1", "name": "Ali"},
    "u2": {"id": "u2", "name": "Sara"},
}
```

### Requirements

* If a record is missing `id`, raise `ValueError`.
* Explain why this improves lookup performance.

---

# Section 3 — Functions And Validation

---

## Exercise 36 — Validate User Payload

**Difficulty:** Medium

Validate a user dictionary.

```python
def validate_user_payload(payload: dict) -> bool:
    pass
```

### Required fields

```text
name
email
role
```

### Example

```python
validate_user_payload({
    "name": "Abubaker",
    "email": "abubaker@example.com",
    "role": "engineer",
})
```

Expected output:

```python
True
```

### Requirements

* Return `False` if any required field is missing.
* Return `False` if email does not contain `@`.
* Keep the function simple and readable.

---

## Exercise 37 — Safe Get Nested Value

**Difficulty:** Medium

Safely get a nested value from a dictionary.

```python
def get_nested(data: dict, keys: list[str], default=None):
    pass
```

### Example

```python
data = {
    "metadata": {
        "source": "core-python"
    }
}

get_nested(data, ["metadata", "source"])
```

Expected output:

```python
"core-python"
```

### Missing example

```python
get_nested(data, ["metadata", "page"], default="unknown")
```

Expected output:

```python
"unknown"
```

---

## Exercise 38 — Retry Function

**Difficulty:** Medium

Write a simple retry function.

```python
def retry(func, attempts: int = 3):
    pass
```

### Requirements

* Call `func`.
* If it fails, retry up to `attempts`.
* If all attempts fail, raise the last exception.
* If `attempts <= 0`, raise `ValueError`.

### Think About

* Why should you not silently swallow exceptions?
* What would you add in production?

---

## Exercise 39 — Apply Transform Pipeline

**Difficulty:** Medium

Apply multiple functions to a value in sequence.

```python
def apply_pipeline(value, functions: list):
    pass
```

### Example

```python
def strip_text(text):
    return text.strip()

def lower_text(text):
    return text.lower()

apply_pipeline("  HELLO  ", [strip_text, lower_text])
```

Expected output:

```python
"hello"
```

### Requirements

* Functions should run in order.
* Return the final transformed value.

---

## Exercise 40 — Create Prefix Function

**Difficulty:** Medium

Use closure to create a prefixing function.

```python
def make_prefixer(prefix: str):
    pass
```

### Example

```python
error = make_prefixer("ERROR: ")
error("Invalid input")
```

Expected output:

```python
"ERROR: Invalid input"
```

### Requirements

* Return an inner function.
* Explain why this is a closure.

---

# Section 4 — Debugging And Edge Cases

---

## Exercise 41 — Fix Empty Max Bug

**Difficulty:** Easy

Fix this function:

```python
def broken_max(nums):
    maximum = nums[0]

    for num in nums:
        if num > maximum:
            maximum = num

    return maximum
```

### Requirement

* If `nums` is empty, raise `ValueError`.

---

## Exercise 42 — Fix Shared Nested List Bug

**Difficulty:** Easy

Fix this matrix creation:

```python
matrix = [[0] * 3] * 3
```

### Requirement

Create a 3x3 matrix with independent rows.

---

## Exercise 43 — Remove None Values

**Difficulty:** Medium

Remove keys with `None` values from a dictionary.

```python
def remove_none_values(data: dict) -> dict:
    pass
```

### Example

```python
remove_none_values({
    "name": "Abubaker",
    "age": None,
    "country": "UAE",
})
```

Expected output:

```python
{
    "name": "Abubaker",
    "country": "UAE",
}
```

### Requirement

* Do not modify the dictionary while iterating over it directly.
* Return a new dictionary.

---

## Exercise 44 — Safe Normalize Text

**Difficulty:** Medium

Normalize text safely.

```python
def safe_normalize_text(text: str | None) -> str:
    pass
```

### Requirements

* If text is `None`, return an empty string.
* Otherwise:

  * strip leading/trailing spaces
  * lowercase
  * collapse repeated spaces

### Example

```python
safe_normalize_text("  Python   FUNCTIONS ")
```

Expected output:

```python
"python functions"
```

---

## Exercise 45 — Debug Wrong Initial Value

**Difficulty:** Medium

Fix this function:

```python
def max_value(nums):
    best = 0

    for num in nums:
        if num > best:
            best = num

    return best
```

### Problem

This fails for:

```python
[-5, -2, -9]
```

Expected output:

```python
-2
```

### Requirement

* Handle all-negative lists.
* If list is empty, raise `ValueError`.

---

# Section 5 — Performance And AI Engineering

---

## Exercise 46 — Optimize Permission Filtering

**Difficulty:** Medium

Optimize this function:

```python
def filter_allowed_documents(documents, allowed_ids):
    result = []

    for doc in documents:
        if doc["id"] in allowed_ids:
            result.append(doc)

    return result
```

### Requirement

* Assume `allowed_ids` is a large list.
* Improve lookup performance.
* Explain time complexity before and after.

---

## Exercise 47 — Deduplicate Search Results

**Difficulty:** Medium

Deduplicate search results by `document_id`.

```python
def deduplicate_results(results: list[dict]) -> list[dict]:
    pass
```

### Example

```python
deduplicate_results([
    {"document_id": "d1", "score": 0.9},
    {"document_id": "d2", "score": 0.8},
    {"document_id": "d1", "score": 0.7},
])
```

Expected output:

```python
[
    {"document_id": "d1", "score": 0.9},
    {"document_id": "d2", "score": 0.8},
]
```

### Requirements

* Preserve first occurrence.
* Use a set.
* Useful for RAG retrieval pipelines.

---

## Exercise 48 — Top K Search Results

**Difficulty:** Medium

Return the top `k` search results by score.

```python
def top_k_results(results: list[dict], k: int) -> list[dict]:
    pass
```

### Example

```python
top_k_results([
    {"id": "doc1", "score": 0.7},
    {"id": "doc2", "score": 0.95},
    {"id": "doc3", "score": 0.8},
], 2)
```

Expected output:

```python
[
    {"id": "doc2", "score": 0.95},
    {"id": "doc3", "score": 0.8},
]
```

### Requirements

* If `k <= 0`, return an empty list.
* You may use sorting.
* Think about when heap is better.

---

## Exercise 49 — Batch Embedding Requests

**Difficulty:** Medium

Create batches for embedding requests.

```python
def create_embedding_batches(texts: list[str], batch_size: int) -> list[list[str]]:
    pass
```

### Example

```python
create_embedding_batches(["a", "b", "c", "d", "e"], 2)
```

Expected output:

```python
[
    ["a", "b"],
    ["c", "d"],
    ["e"],
]
```

### Requirements

* If `batch_size <= 0`, raise `ValueError`.
* Preserve order.
* This is a real AI engineering pattern.

---

## Exercise 50 — Mini RAG Preprocessing Pipeline

**Difficulty:** Hard

Build a mini preprocessing pipeline for text chunks.

```python
def rag_preprocess(chunks: list[str], batch_size: int) -> list[list[str]]:
    pass
```

### Requirements

The pipeline should:

1. Normalize each chunk:

   * strip spaces
   * lowercase
   * collapse repeated spaces
2. Remove empty chunks.
3. Deduplicate normalized chunks while preserving order.
4. Split into batches.

### Example

```python
rag_preprocess([
    " Python   Functions ",
    "python functions",
    "",
    " Dictionaries ",
    "DICTIONARIES"
], batch_size=2)
```

Expected output:

```python
[
    ["python functions", "dictionaries"]
]
```

### Think About

* Why normalize before deduplication?
* Why remove empty chunks?
* Why batch before embedding?
* What is the time complexity?
* What is the space complexity?

---

# Completion Checklist

Before checking the solutions, make sure you can answer:

* [ ] Did I handle empty input?
* [ ] Did I handle duplicates?
* [ ] Did I choose the right data structure?
* [ ] Did I avoid unnecessary O(n²) behavior?
* [ ] Did I validate invalid input?
* [ ] Did I explain time complexity?
* [ ] Did I explain space complexity?
* [ ] Did I write readable code?
* [ ] Could this be used in production?
* [ ] Could I explain this in an interview?

---

# Recommended Self-Review Questions

For every solution, ask:

```text
What is the brute-force version?
What is the optimized version?
What data structure improves it?
What edge case could break it?
What would change in production?
```

---

# Final Note

Exercises 26–50 are designed to connect Core Python with real AI engineering patterns.

Especially important exercises:

```text
46 — Permission filtering
47 — Deduplicate search results
48 — Top K search results
49 — Batch embedding requests
50 — Mini RAG preprocessing pipeline
```

These are not only interview exercises.

They are real production patterns.
