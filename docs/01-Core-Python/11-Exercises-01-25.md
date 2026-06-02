# Core Python Exercises 01–25

> Practice problems for Core Python: memory model, lists, tuples, sets, dictionaries, functions, complexity, debugging, and performance.

---

# How To Use This File

Do not read the solutions first.

For each exercise:

1. Read the problem carefully.
2. Write the function yourself.
3. Test with the provided examples.
4. Analyze time complexity.
5. Analyze space complexity.
6. Compare with the solution later.

The goal is not only to get the correct output.

The goal is to explain:

```text
Why this solution works
What data structure is used
What the complexity is
What tradeoff is made
```

---

# Difficulty Guide

| Level  | Meaning                                   |
| ------ | ----------------------------------------- |
| Easy   | Core syntax and simple logic              |
| Medium | Common interview patterns                 |
| Hard   | Requires deeper reasoning or optimization |

---

# Section 1 — Lists

---

## Exercise 01 — Find Maximum

**Difficulty:** Easy

Write a function that returns the maximum number in a list.

```python
def find_max(nums: list[int]) -> int:
    pass
```

### Example

```python
find_max([3, 7, 2, 9, 1])
```

Expected output:

```python
9
```

### Requirements

* Do not use `max()`.
* Handle negative numbers.
* If the list is empty, raise `ValueError`.

### Think About

* What should the initial maximum be?
* Why is using `0` as the initial value dangerous?

---

## Exercise 02 — Find Minimum

**Difficulty:** Easy

Write a function that returns the minimum number in a list.

```python
def find_min(nums: list[int]) -> int:
    pass
```

### Example

```python
find_min([3, 7, 2, 9, 1])
```

Expected output:

```python
1
```

### Requirements

* Do not use `min()`.
* Handle negative numbers.
* If the list is empty, raise `ValueError`.

---

## Exercise 03 — Sum List

**Difficulty:** Easy

Write a function that returns the sum of all numbers in a list.

```python
def list_sum(nums: list[int]) -> int:
    pass
```

### Example

```python
list_sum([1, 2, 3, 4])
```

Expected output:

```python
10
```

### Requirements

* Do not use `sum()`.
* Empty list should return `0`.

---

## Exercise 04 — Reverse List In Place

**Difficulty:** Easy

Write a function that reverses a list in place.

```python
def reverse_in_place(nums: list[int]) -> list[int]:
    pass
```

### Example

```python
nums = [1, 2, 3, 4]
reverse_in_place(nums)
```

Expected output:

```python
[4, 3, 2, 1]
```

### Requirements

* Do not use `reverse()`.
* Do not use slicing `[::-1]`.
* Modify the original list.
* Use the two-pointer pattern.

### Think About

* What is the difference between returning a new reversed list and reversing in place?
* What is the space complexity?

---

## Exercise 05 — Count Even Numbers

**Difficulty:** Easy

Write a function that counts how many even numbers exist in a list.

```python
def count_even(nums: list[int]) -> int:
    pass
```

### Example

```python
count_even([1, 2, 3, 4, 5, 6])
```

Expected output:

```python
3
```

---

## Exercise 06 — Remove Duplicates Preserve Order

**Difficulty:** Medium

Write a function that removes duplicates while preserving the original order.

```python
def remove_duplicates_ordered(items: list[int]) -> list[int]:
    pass
```

### Example

```python
remove_duplicates_ordered([1, 2, 2, 3, 1, 4])
```

Expected output:

```python
[1, 2, 3, 4]
```

### Requirements

* Preserve first occurrence.
* Do not use `list(set(items))` because it does not preserve order reliably.
* Use a `set` for efficient lookup.

### Think About

* Why is checking `item in result` less efficient?
* What is the time-space tradeoff?

---

## Exercise 07 — Rotate List Right

**Difficulty:** Medium

Rotate a list to the right by `k` steps.

```python
def rotate_right(nums: list[int], k: int) -> list[int]:
    pass
```

### Example

```python
rotate_right([1, 2, 3, 4, 5], 2)
```

Expected output:

```python
[4, 5, 1, 2, 3]
```

### Requirements

* Handle `k > len(nums)`.
* Handle empty list.
* Return a new list.

### Think About

* Why do we use `k % len(nums)`?

---

## Exercise 08 — Merge Two Sorted Lists

**Difficulty:** Medium

Merge two sorted lists into one sorted list.

```python
def merge_sorted(a: list[int], b: list[int]) -> list[int]:
    pass
```

### Example

```python
merge_sorted([1, 3, 5], [2, 4, 6])
```

Expected output:

```python
[1, 2, 3, 4, 5, 6]
```

### Requirements

* Do not use `sorted(a + b)`.
* Use two pointers.
* Preserve duplicates.

---

## Exercise 09 — Product Except Self

**Difficulty:** Hard

Given a list of integers, return a list where each position contains the product of all other numbers except itself.

```python
def product_except_self(nums: list[int]) -> list[int]:
    pass
```

### Example

```python
product_except_self([1, 2, 3, 4])
```

Expected output:

```python
[24, 12, 8, 6]
```

### Requirements

* Do not use division.
* Aim for O(n) time.
* Try to solve with O(1) extra space excluding the output list.

---

## Exercise 10 — Maximum Subarray Sum

**Difficulty:** Hard

Implement Kadane's Algorithm.

```python
def max_subarray_sum(nums: list[int]) -> int:
    pass
```

### Example

```python
max_subarray_sum([-2, 1, -3, 4, -1, 2, 1, -5, 4])
```

Expected output:

```python
6
```

Explanation:

```text
[4, -1, 2, 1] has sum 6
```

### Requirements

* Handle all-negative lists.
* If list is empty, raise `ValueError`.

---

# Section 2 — Tuples

---

## Exercise 11 — Return Min And Max

**Difficulty:** Easy

Write a function that returns both minimum and maximum values from a list.

```python
def min_max(nums: list[int]) -> tuple[int, int]:
    pass
```

### Example

```python
min_max([3, 1, 9, 2])
```

Expected output:

```python
(1, 9)
```

### Requirements

* Do not use `min()` or `max()`.
* Return a tuple.
* If the list is empty, raise `ValueError`.

---

## Exercise 12 — Swap Variables

**Difficulty:** Easy

Use tuple unpacking to swap two variables.

```python
def swap(a: int, b: int) -> tuple[int, int]:
    pass
```

### Example

```python
swap(10, 20)
```

Expected output:

```python
(20, 10)
```

---

## Exercise 13 — Build Cache Key

**Difficulty:** Medium

Build a tuple cache key for an embedding request.

```python
def build_cache_key(model_name: str, version: str, text_hash: str) -> tuple[str, str, str]:
    pass
```

### Example

```python
build_cache_key("embedding-small", "v1", "abc123")
```

Expected output:

```python
("embedding-small", "v1", "abc123")
```

### Think About

* Why is a tuple better than a list here?
* Why should a cache key be immutable?

---

## Exercise 14 — Sort Search Results

**Difficulty:** Medium

Given search results as tuples:

```python
(document_id, score)
```

Sort them by score descending.

```python
def sort_results(results: list[tuple[str, float]]) -> list[tuple[str, float]]:
    pass
```

### Example

```python
sort_results([
    ("doc1", 0.7),
    ("doc2", 0.95),
    ("doc3", 0.8),
])
```

Expected output:

```python
[
    ("doc2", 0.95),
    ("doc3", 0.8),
    ("doc1", 0.7),
]
```

---

# Section 3 — Sets

---

## Exercise 15 — Has Duplicates

**Difficulty:** Easy

Write a function that returns `True` if a list contains duplicates.

```python
def has_duplicates(items: list[int]) -> bool:
    pass
```

### Example

```python
has_duplicates([1, 2, 3, 2])
```

Expected output:

```python
True
```

### Requirements

* Use a set.
* Aim for O(n) time.

---

## Exercise 16 — Common Elements

**Difficulty:** Easy

Return common elements between two lists.

```python
def common_elements(a: list[int], b: list[int]) -> list[int]:
    pass
```

### Example

```python
common_elements([1, 2, 3], [2, 3, 4])
```

Expected output:

```python
[2, 3]
```

### Notes

Order does not matter.

---

## Exercise 17 — Missing IDs

**Difficulty:** Medium

Given expected IDs and actual IDs, return the missing IDs.

```python
def missing_ids(expected: list[str], actual: list[str]) -> set[str]:
    pass
```

### Example

```python
missing_ids(
    ["u1", "u2", "u3"],
    ["u1", "u3"]
)
```

Expected output:

```python
{"u2"}
```

---

## Exercise 18 — Deduplicate RAG Chunks

**Difficulty:** Medium

Normalize and deduplicate text chunks.

```python
def deduplicate_chunks(chunks: list[str]) -> list[str]:
    pass
```

### Example

```python
deduplicate_chunks([
    "Python Functions",
    " python   functions ",
    "Dictionaries"
])
```

Expected output:

```python
[
    "Python Functions",
    "Dictionaries"
]
```

### Requirements

Two chunks should be considered the same if, after normalization:

* leading/trailing spaces are removed
* text is lowercased
* repeated spaces are collapsed

### Think About

* Why is raw `set(chunks)` not enough?
* Why do we keep the original chunk text but compare using normalized text?

---

# Section 4 — Dictionaries

---

## Exercise 19 — Character Frequency

**Difficulty:** Easy

Count character frequencies in a string.

```python
def char_frequency(text: str) -> dict[str, int]:
    pass
```

### Example

```python
char_frequency("banana")
```

Expected output:

```python
{
    "b": 1,
    "a": 3,
    "n": 2
}
```

---

## Exercise 20 — Word Frequency

**Difficulty:** Easy

Count word frequencies in a sentence.

```python
def word_frequency(sentence: str) -> dict[str, int]:
    pass
```

### Example

```python
word_frequency("AI is good and AI is useful")
```

Expected output:

```python
{
    "ai": 2,
    "is": 2,
    "good": 1,
    "and": 1,
    "useful": 1
}
```

### Requirements

* Lowercase words.
* Split by whitespace.

---

## Exercise 21 — First Non-Repeating Character

**Difficulty:** Medium

Find the first character that appears only once.

```python
def first_non_repeating_char(text: str) -> str | None:
    pass
```

### Example

```python
first_non_repeating_char("swiss")
```

Expected output:

```python
"w"
```

### Requirements

* Return `None` if no unique character exists.
* Preserve original order.

---

## Exercise 22 — Group Words By Length

**Difficulty:** Medium

Group words by their length.

```python
def group_words_by_length(words: list[str]) -> dict[int, list[str]]:
    pass
```

### Example

```python
group_words_by_length(["AI", "ML", "RAG", "Python"])
```

Expected output:

```python
{
    2: ["AI", "ML"],
    3: ["RAG"],
    6: ["Python"]
}
```

---

## Exercise 23 — Two Sum

**Difficulty:** Medium

Given a list of numbers and a target, return the indexes of two numbers that add up to the target.

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    pass
```

### Example

```python
two_sum([2, 7, 11, 15], 9)
```

Expected output:

```python
[0, 1]
```

### Requirements

* Use a dictionary.
* Return an empty list if no pair exists.
* Aim for O(n) time.

---

# Section 5 — Functions, Debugging, Performance

---

## Exercise 24 — Safe Average

**Difficulty:** Easy

Write a safe average function.

```python
def average(nums: list[float]) -> float:
    pass
```

### Example

```python
average([10, 20, 30])
```

Expected output:

```python
20.0
```

### Requirements

* If the list is empty, raise `ValueError`.
* Do not silently return `0` for empty input.

### Think About

* Why is returning `0` dangerous?
* Why is explicit failure better here?

---

## Exercise 25 — Build Embedding Cache Key

**Difficulty:** Medium

Build a safe embedding cache key.

```python
def build_embedding_cache_key(
    model_name: str,
    embedding_version: str,
    text: str
) -> tuple[str, str, str]:
    pass
```

### Requirements

The function should:

1. Normalize the text.
2. Hash the normalized text with SHA-256.
3. Return:

```python
(model_name, embedding_version, text_hash)
```

### Example

```python
build_embedding_cache_key(
    "embedding-small",
    "v1",
    "  Python   Functions "
)
```

Expected output shape:

```python
("embedding-small", "v1", "<sha256_hash>")
```

### Think About

* Why should the raw text not be used directly as the key?
* Why should the model name and version be included?
* Why is a tuple appropriate for the return value?

---

# Completion Checklist

Before opening the solutions file, make sure you can answer:

* [ ] What data structure did I use?
* [ ] Why did I choose that structure?
* [ ] What is the time complexity?
* [ ] What is the space complexity?
* [ ] What edge cases did I handle?
* [ ] Could this fail in production?
* [ ] Is my function clear and testable?

---

# Recommended Practice Flow

For each exercise:

```text
Attempt
↓
Test examples
↓
Add edge cases
↓
Analyze complexity
↓
Compare with solution
↓
Rewrite without looking
```

---

# Edge Cases To Always Consider

```text
empty list
single item
duplicates
negative numbers
None values
missing dictionary keys
large input
case sensitivity
extra spaces
invalid input
```

---

# Final Note

These first 25 exercises focus on Core Python fundamentals.

They are not random problems.

They are designed to train the patterns that appear repeatedly in:

* Python coding interviews
* backend services
* data pipelines
* ML preprocessing
* RAG systems
* AI engineering production code

Do not rush them.

Solve them deeply.
