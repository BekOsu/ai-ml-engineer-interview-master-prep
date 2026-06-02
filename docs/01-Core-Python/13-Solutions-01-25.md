# Core Python Solutions 01–25

> Detailed solutions for Core Python Exercises 01–25.

---

# How To Use This File

Do not only copy the solutions.

For each exercise, study:

1. Why the solution works
2. Which data structure is used
3. Time complexity
4. Space complexity
5. Edge cases
6. Production considerations

A strong interview answer should explain both the code and the tradeoff.

---

# Exercise 01 — Find Maximum

## Solution

```python
def find_max(nums: list[int]) -> int:
    if not nums:
        raise ValueError("nums must not be empty")

    maximum = nums[0]

    for num in nums:
        if num > maximum:
            maximum = num

    return maximum
```

## Explanation

We initialize `maximum` with the first element, not `0`.

This is important because the list may contain only negative numbers.

Example:

```python
find_max([-5, -2, -9])
```

Correct output:

```python
-2
```

If we initialized `maximum = 0`, the result would incorrectly be `0`.

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 02 — Find Minimum

## Solution

```python
def find_min(nums: list[int]) -> int:
    if not nums:
        raise ValueError("nums must not be empty")

    minimum = nums[0]

    for num in nums:
        if num < minimum:
            minimum = num

    return minimum
```

## Explanation

We scan the list once and keep track of the smallest value seen so far.

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 03 — Sum List

## Solution

```python
def list_sum(nums: list[int]) -> int:
    total = 0

    for num in nums:
        total += num

    return total
```

## Explanation

We start with `0` because it is the identity value for addition.

An empty list returns `0`.

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 04 — Reverse List In Place

## Solution

```python
def reverse_in_place(nums: list[int]) -> list[int]:
    left = 0
    right = len(nums) - 1

    while left < right:
        nums[left], nums[right] = nums[right], nums[left]

        left += 1
        right -= 1

    return nums
```

## Explanation

We use two pointers:

```text
left  -> start of list
right -> end of list
```

At each step, we swap both values and move inward.

This modifies the original list.

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 05 — Count Even Numbers

## Solution

```python
def count_even(nums: list[int]) -> int:
    count = 0

    for num in nums:
        if num % 2 == 0:
            count += 1

    return count
```

## Explanation

A number is even if:

```python
num % 2 == 0
```

We scan the list once.

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 06 — Remove Duplicates Preserve Order

## Solution

```python
def remove_duplicates_ordered(items: list[int]) -> list[int]:
    seen = set()
    result = []

    for item in items:
        if item in seen:
            continue

        seen.add(item)
        result.append(item)

    return result
```

## Explanation

We use:

```python
seen
```

to track values already processed.

We use:

```python
result
```

to preserve order.

Do not use:

```python
list(set(items))
```

because order may not match the original list.

## Complexity

```text
Time:  O(n)
Space: O(n)
```

## Tradeoff

We use extra memory for the `seen` set to get fast O(1) average membership checks.

---

# Exercise 07 — Rotate List Right

## Solution

```python
def rotate_right(nums: list[int], k: int) -> list[int]:
    if not nums:
        return []

    k = k % len(nums)

    return nums[-k:] + nums[:-k]
```

## Explanation

If:

```python
nums = [1, 2, 3, 4, 5]
k = 2
```

Then:

```python
nums[-2:]  -> [4, 5]
nums[:-2]  -> [1, 2, 3]
```

Result:

```python
[4, 5, 1, 2, 3]
```

We use:

```python
k % len(nums)
```

to handle cases where `k` is larger than the list length.

Example:

```python
k = 7
len(nums) = 5
k % 5 = 2
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

Because slicing creates new lists.

---

# Exercise 08 — Merge Two Sorted Lists

## Solution

```python
def merge_sorted(a: list[int], b: list[int]) -> list[int]:
    result = []

    i = 0
    j = 0

    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            result.append(a[i])
            i += 1
        else:
            result.append(b[j])
            j += 1

    result.extend(a[i:])
    result.extend(b[j:])

    return result
```

## Explanation

Because both lists are already sorted, we can merge them using two pointers.

We compare the current values and append the smaller one.

When one list finishes, we append the remaining values from the other list.

## Complexity

```text
Time:  O(n + m)
Space: O(n + m)
```

Where:

```text
n = len(a)
m = len(b)
```

---

# Exercise 09 — Product Except Self

## Solution

```python
def product_except_self(nums: list[int]) -> list[int]:
    n = len(nums)

    result = [1] * n

    prefix = 1

    for i in range(n):
        result[i] = prefix
        prefix *= nums[i]

    suffix = 1

    for i in range(n - 1, -1, -1):
        result[i] *= suffix
        suffix *= nums[i]

    return result
```

## Explanation

We avoid division by using prefix and suffix products.

For each index:

```text
result[i] = product of values before i * product of values after i
```

First pass stores prefix products.

Second pass multiplies suffix products.

Example:

```python
nums = [1, 2, 3, 4]
```

Output:

```python
[24, 12, 8, 6]
```

## Complexity

```text
Time:  O(n)
Space: O(1) extra space excluding output
```

---

# Exercise 10 — Maximum Subarray Sum

## Solution

```python
def max_subarray_sum(nums: list[int]) -> int:
    if not nums:
        raise ValueError("nums must not be empty")

    current = nums[0]
    best = nums[0]

    for num in nums[1:]:
        current = max(num, current + num)
        best = max(best, current)

    return best
```

## Explanation

This is Kadane's Algorithm.

At each number, we ask:

```text
Should I start a new subarray here?
or
Should I extend the previous subarray?
```

The line:

```python
current = max(num, current + num)
```

decides that.

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 11 — Return Min And Max

## Solution

```python
def min_max(nums: list[int]) -> tuple[int, int]:
    if not nums:
        raise ValueError("nums must not be empty")

    minimum = nums[0]
    maximum = nums[0]

    for num in nums:
        if num < minimum:
            minimum = num

        if num > maximum:
            maximum = num

    return minimum, maximum
```

## Explanation

The function returns two values as a tuple.

Usage:

```python
low, high = min_max([3, 1, 9, 2])
```

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 12 — Swap Variables

## Solution

```python
def swap(a: int, b: int) -> tuple[int, int]:
    return b, a
```

## Explanation

Python supports tuple packing and unpacking.

Example:

```python
a, b = b, a
```

No temporary variable is needed.

## Complexity

```text
Time:  O(1)
Space: O(1)
```

---

# Exercise 13 — Build Cache Key

## Solution

```python
def build_cache_key(
    model_name: str,
    version: str,
    text_hash: str
) -> tuple[str, str, str]:
    return model_name, version, text_hash
```

## Explanation

A tuple is appropriate because cache keys should be immutable and hashable.

A list would not work as a dictionary key because lists are mutable and unhashable.

## Complexity

```text
Time:  O(1)
Space: O(1)
```

---

# Exercise 14 — Sort Search Results

## Solution

```python
def sort_results(
    results: list[tuple[str, float]]
) -> list[tuple[str, float]]:
    return sorted(
        results,
        key=lambda item: item[1],
        reverse=True
    )
```

## Explanation

Each item is:

```text
(document_id, score)
```

The score is at index `1`.

We sort descending using:

```python
reverse=True
```

## Complexity

```text
Time:  O(n log n)
Space: O(n)
```

Because `sorted()` returns a new list.

---

# Exercise 15 — Has Duplicates

## Solution

```python
def has_duplicates(items: list[int]) -> bool:
    seen = set()

    for item in items:
        if item in seen:
            return True

        seen.add(item)

    return False
```

## Short Version

```python
def has_duplicates(items: list[int]) -> bool:
    return len(items) != len(set(items))
```

## Explanation

The set tracks seen values.

If we see the same value again, the list contains duplicates.

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# Exercise 16 — Common Elements

## Solution

```python
def common_elements(a: list[int], b: list[int]) -> list[int]:
    return list(set(a) & set(b))
```

## Explanation

We convert both lists to sets and use intersection.

Order is not guaranteed.

## Complexity

```text
Time:  O(n + m)
Space: O(n + m)
```

---

# Exercise 17 — Missing IDs

## Solution

```python
def missing_ids(expected: list[str], actual: list[str]) -> set[str]:
    return set(expected) - set(actual)
```

## Explanation

Set difference returns values in `expected` that are not in `actual`.

## Complexity

```text
Time:  O(n + m)
Space: O(n + m)
```

---

# Exercise 18 — Deduplicate RAG Chunks

## Solution

```python
def normalize_text(text: str) -> str:
    return " ".join(
        text.strip().lower().split()
    )


def deduplicate_chunks(chunks: list[str]) -> list[str]:
    seen = set()
    result = []

    for chunk in chunks:
        key = normalize_text(chunk)

        if key in seen:
            continue

        seen.add(key)
        result.append(chunk)

    return result
```

## Explanation

We compare chunks using normalized text but keep the original first version.

This means:

```text
"Python Functions"
" python   functions "
```

are considered duplicates.

## Complexity

```text
Time:  O(n * k)
Space: O(n * k)
```

Where:

```text
n = number of chunks
k = average chunk length
```

---

# Exercise 19 — Character Frequency

## Solution

```python
def char_frequency(text: str) -> dict[str, int]:
    freq = {}

    for char in text:
        freq[char] = freq.get(char, 0) + 1

    return freq
```

## Explanation

We use a dictionary where:

```text
key   = character
value = count
```

## Complexity

```text
Time:  O(n)
Space: O(k)
```

Where `k` is the number of unique characters.

---

# Exercise 20 — Word Frequency

## Solution

```python
def word_frequency(sentence: str) -> dict[str, int]:
    freq = {}

    for word in sentence.lower().split():
        freq[word] = freq.get(word, 0) + 1

    return freq
```

## Explanation

We lowercase first so:

```text
AI
ai
Ai
```

are counted as the same word.

## Complexity

```text
Time:  O(n)
Space: O(k)
```

Where:

```text
n = number of words
k = number of unique words
```

---

# Exercise 21 — First Non-Repeating Character

## Solution

```python
def first_non_repeating_char(text: str) -> str | None:
    freq = {}

    for char in text:
        freq[char] = freq.get(char, 0) + 1

    for char in text:
        if freq[char] == 1:
            return char

    return None
```

## Explanation

We need two passes:

1. Count characters
2. Return the first character with count `1`

This preserves original order.

## Complexity

```text
Time:  O(n)
Space: O(k)
```

---

# Exercise 22 — Group Words By Length

## Solution

```python
def group_words_by_length(words: list[str]) -> dict[int, list[str]]:
    groups = {}

    for word in words:
        length = len(word)

        if length not in groups:
            groups[length] = []

        groups[length].append(word)

    return groups
```

## Alternative With defaultdict

```python
from collections import defaultdict

def group_words_by_length(words: list[str]) -> dict[int, list[str]]:
    groups = defaultdict(list)

    for word in words:
        groups[len(word)].append(word)

    return dict(groups)
```

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# Exercise 23 — Two Sum

## Solution

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

## Explanation

For each number, we check whether the needed complement has already appeared.

Example:

```text
target = 9
current num = 7
diff = 2
```

If `2` was seen before, we found the answer.

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

# Exercise 24 — Safe Average

## Solution

```python
def average(nums: list[float]) -> float:
    if not nums:
        raise ValueError("nums must not be empty")

    return sum(nums) / len(nums)
```

## Explanation

Returning `0` for an empty list can hide data problems.

Example:

```text
No scores available
```

is not the same as:

```text
Average score is 0
```

Explicit failure is safer here.

## Complexity

```text
Time:  O(n)
Space: O(1)
```

---

# Exercise 25 — Build Embedding Cache Key

## Solution

```python
import hashlib


def normalize_text(text: str) -> str:
    return " ".join(
        text.strip().lower().split()
    )


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

## Explanation

A safe embedding cache key should include:

```text
model name
embedding version
normalized text hash
```

Why?

Because the same text embedded by different models may produce different vectors.

Using raw text directly as a key may be:

* too large
* inconsistent due to spaces/case
* risky for memory
* unsafe if text contains sensitive data

A tuple is appropriate because it is immutable and hashable.

## Complexity

```text
Time:  O(k)
Space: O(k)
```

Where `k` is the length of the text.

---

# Final Review

If you completed Exercises 01–25, you practiced:

* list scanning
* two pointers
* set membership
* dictionary counting
* grouping
* tuple returns
* cache key design
* safe validation
* RAG-style deduplication
* complexity analysis

These are core patterns for both Python interviews and AI engineering work.

---

# Mastery Checklist

You are ready for Exercises 26–50 if you can explain:

* [ ] Why `find_max` should not start with `0`
* [ ] Why reversing in place is O(1) space
* [ ] Why set-based deduplication is faster than list lookup
* [ ] Why tuple cache keys are useful
* [ ] Why dictionary-based Two Sum is O(n)
* [ ] Why normalized text matters in RAG pipelines
* [ ] Why safe average should reject empty input
* [ ] Why embedding cache keys must include model/version
