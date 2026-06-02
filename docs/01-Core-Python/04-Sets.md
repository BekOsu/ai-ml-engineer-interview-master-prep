# Python Sets

> Hash-based collections for uniqueness, fast membership checks, deduplication, and interview optimization patterns.

---

# Why Sets Matter

Sets are one of the most important Python data structures for interviews.

They are simple on the surface, but they unlock many efficient solutions.

A lot of beginner solutions use lists where sets would be better.

Example:

```python
if user_id in users:
    ...
```

If `users` is a list:

```text
O(n)
```

If `users` is a set:

```text
O(1) average case
```

That difference becomes huge when working with:

* Millions of users
* Large logs
* Duplicate records
* RAG document IDs
* Permission checks
* Feature flags
* Data cleaning pipelines

Senior interviewers often test whether you know when to replace a list with a set.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain what a set is
* Explain why sets are fast for membership checks
* Explain hashing
* Explain uniqueness
* Use set operations: union, intersection, difference
* Remove duplicates efficiently
* Solve common interview problems with sets
* Compare set vs list vs dict
* Explain set limitations
* Use sets in backend and AI engineering systems

---

# What Is A Set?

A set is an unordered collection of unique elements.

Example:

```python
numbers = {1, 2, 3, 4}
```

Sets automatically remove duplicates.

```python
numbers = {1, 2, 2, 3, 3, 3}

print(numbers)
```

Output:

```python
{1, 2, 3}
```

Important properties:

* Unordered
* Mutable
* Unique values only
* Fast membership checks
* Elements must be hashable

---

# Creating Sets

## Non-empty Set

```python
items = {1, 2, 3}
```

## Empty Set

Important mistake:

```python
items = {}
```

This creates a dictionary, not a set.

Correct:

```python
items = set()
```

Check:

```python
print(type({}))
print(type(set()))
```

Output:

```python
<class 'dict'>
<class 'set'>
```

---

# Visual Model

A list stores values by position:

```text
List

Index:   0   1   2   3
Value:  10  20  30  40
```

A set stores values using hashing:

```text
Set

value ──hash()──► bucket location
```

Conceptually:

```text
"Ali"   ──hash──► bucket 3
"Sara"  ──hash──► bucket 7
"Omar"  ──hash──► bucket 2
```

This is why lookup can be very fast.

---

# Membership Check

This is the main reason sets are important.

```python
allowed_users = {"u1", "u2", "u3"}

if "u2" in allowed_users:
    print("Allowed")
```

Average complexity:

```text
O(1)
```

For lists:

```python
allowed_users = ["u1", "u2", "u3"]

if "u2" in allowed_users:
    print("Allowed")
```

Complexity:

```text
O(n)
```

---

# Why Set Lookup Is Fast

Sets use hash tables.

When you check:

```python
x in my_set
```

Python computes:

```python
hash(x)
```

Then jumps near the expected memory location.

It does not scan every item like a list.

Conceptually:

```text
value
  │
  ▼
hash(value)
  │
  ▼
bucket index
  │
  ▼
check item
```

This gives average O(1) lookup.

---

# Hashability

Set elements must be hashable.

Hashable means:

* The object has a stable hash value
* The hash does not change during the object's lifetime
* The object can be compared for equality

Valid set elements:

```python
items = {1, 2, "hello", (10, 20)}
```

Invalid:

```python
items = {[1, 2], [3, 4]}
```

Error:

```python
TypeError: unhashable type: 'list'
```

Lists are mutable, so they cannot safely be stored in a set.

---

# Valid And Invalid Set Elements

## Valid

```python
valid = {
    10,
    3.14,
    "python",
    True,
    (1, 2)
}
```

## Invalid

```python
invalid = {
    [1, 2],
    {"name": "Ali"},
    {1, 2}
}
```

Why invalid?

Because:

```text
list
dict
set
```

are mutable and unhashable.

---

# Set Operations

Sets are powerful because they support mathematical operations.

---

## Union

Union combines two sets.

```python
a = {1, 2, 3}
b = {3, 4, 5}

result = a | b

print(result)
```

Output:

```python
{1, 2, 3, 4, 5}
```

Alternative:

```python
a.union(b)
```

---

## Intersection

Intersection returns common elements.

```python
a = {1, 2, 3}
b = {3, 4, 5}

result = a & b

print(result)
```

Output:

```python
{3}
```

Alternative:

```python
a.intersection(b)
```

---

## Difference

Difference returns elements in `a` but not in `b`.

```python
a = {1, 2, 3}
b = {3, 4, 5}

result = a - b

print(result)
```

Output:

```python
{1, 2}
```

Alternative:

```python
a.difference(b)
```

---

## Symmetric Difference

Symmetric difference returns elements that exist in either set, but not both.

```python
a = {1, 2, 3}
b = {3, 4, 5}

result = a ^ b

print(result)
```

Output:

```python
{1, 2, 4, 5}
```

Alternative:

```python
a.symmetric_difference(b)
```

---

# Common Set Methods

## add()

```python
items = set()

items.add("python")
```

---

## remove()

```python
items.remove("python")
```

Raises an error if item does not exist.

---

## discard()

```python
items.discard("python")
```

Does not raise an error if item does not exist.

---

## pop()

```python
item = items.pop()
```

Removes and returns an arbitrary item.

Because sets are unordered, you should not depend on which item is removed.

---

## clear()

```python
items.clear()
```

Removes all items.

---

# Set Complexity Table

| Operation        | Average Complexity     |
| ---------------- | ---------------------- |
| Add              | O(1)                   |
| Remove           | O(1)                   |
| Discard          | O(1)                   |
| Membership check | O(1)                   |
| Iteration        | O(n)                   |
| Union            | O(len(a) + len(b))     |
| Intersection     | O(min(len(a), len(b))) |
| Difference       | O(len(a))              |

Important:

These are average-case complexities.

Hash collisions can degrade performance, but average-case behavior is excellent.

---

# Sets Are Unordered

Sets do not preserve meaningful order.

Example:

```python
items = {"banana", "apple", "orange"}

print(items)
```

Output may appear in different order.

Do not write code that depends on set ordering.

Bad:

```python
first = list(items)[0]
```

This is unreliable if order matters.

Use a list if order matters.

---

# Deduplication

A common use case.

```python
nums = [1, 1, 2, 2, 3, 3]

unique = set(nums)

print(unique)
```

Output:

```python
{1, 2, 3}
```

If you need a list:

```python
unique_list = list(set(nums))
```

But this may not preserve order.

---

# Deduplication While Preserving Order

Modern Python dictionaries preserve insertion order, so this pattern is common:

```python
nums = [1, 1, 2, 2, 3, 3]

unique = list(dict.fromkeys(nums))

print(unique)
```

Output:

```python
[1, 2, 3]
```

Alternative manual solution:

```python
def deduplicate_preserve_order(items):
    seen = set()
    result = []

    for item in items:
        if item not in seen:
            seen.add(item)
            result.append(item)

    return result
```

This is an important interview pattern.

---

# Common Interview Pattern: Seen Set

Sets are frequently used to track what has already been seen.

Example:

```python
def has_duplicate(nums):
    seen = set()

    for num in nums:
        if num in seen:
            return True

        seen.add(num)

    return False
```

Complexity:

```text
Time: O(n)
Space: O(n)
```

Without a set, the naive solution would be O(n²).

---

# Common Interview Pattern: Intersection

```python
def common_items(a, b):
    return list(set(a) & set(b))
```

Example:

```python
common_items([1, 2, 3], [2, 3, 4])
```

Output:

```python
[2, 3]
```

Order is not guaranteed.

---

# Common Interview Pattern: Missing Elements

```python
def missing_items(expected, actual):
    return set(expected) - set(actual)
```

Example:

```python
expected = {"user_1", "user_2", "user_3"}
actual = {"user_1", "user_3"}

print(missing_items(expected, actual))
```

Output:

```python
{"user_2"}
```

---

# Set vs List

| Feature           | List                | Set                   |
| ----------------- | ------------------- | --------------------- |
| Ordered           | Yes                 | No                    |
| Allows duplicates | Yes                 | No                    |
| Membership check  | O(n)                | O(1) average          |
| Index access      | O(1)                | No                    |
| Best for          | Ordered collections | Uniqueness and lookup |

---

# Set vs Dictionary

A set is like a dictionary with only keys.

```python
users = {"u1", "u2", "u3"}
```

A dictionary stores key-value pairs.

```python
users = {
    "u1": "Ali",
    "u2": "Sara"
}
```

Use set when you only care whether something exists.

Use dict when you need to map a key to a value.

---

# frozenset

A `frozenset` is an immutable set.

```python
items = frozenset([1, 2, 3])
```

You cannot add or remove elements:

```python
items.add(4)
```

Error:

```python
AttributeError
```

Why useful?

Because `frozenset` is hashable.

This means it can be used as a dictionary key or stored inside another set.

Example:

```python
key = frozenset(["read", "write"])

permissions_cache = {
    key: "admin-like-access"
}
```

---

# Production Engineering Notes

Sets are commonly used in backend systems for:

* Permission checks
* Feature flag checks
* Deduplication
* Rate limiting
* Filtering
* Tracking processed IDs
* Preventing repeated work

Example:

```python
processed_ids = set()

for event in events:
    if event.id in processed_ids:
        continue

    process_event(event)
    processed_ids.add(event.id)
```

This prevents duplicate processing.

---

# AI Engineering Examples

## Example 1: Avoid Duplicate Chunks

In a RAG pipeline, you may need to avoid embedding duplicate chunks.

```python
seen_chunks = set()
unique_chunks = []

for chunk in chunks:
    normalized = chunk.strip().lower()

    if normalized in seen_chunks:
        continue

    seen_chunks.add(normalized)
    unique_chunks.append(chunk)
```

This avoids wasting embedding API calls.

---

## Example 2: Track Retrieved Document IDs

```python
retrieved_doc_ids = set()

for result in vector_results:
    if result.document_id in retrieved_doc_ids:
        continue

    retrieved_doc_ids.add(result.document_id)
    final_results.append(result)
```

Useful when results come from multiple retrievers:

* Vector retriever
* Keyword retriever
* Metadata filter
* Reranker

---

## Example 3: Permission Filtering

```python
allowed_doc_ids = set(user_allowed_documents)

visible_results = [
    result
    for result in search_results
    if result.document_id in allowed_doc_ids
]
```

This makes permission filtering efficient.

---

# Common Mistakes

## Mistake 1: Creating Empty Set Incorrectly

Wrong:

```python
items = {}
```

This creates a dictionary.

Correct:

```python
items = set()
```

---

## Mistake 2: Expecting Set Order

Wrong assumption:

```python
items = {"a", "b", "c"}

first = list(items)[0]
```

Do not depend on this order.

---

## Mistake 3: Putting Lists Inside Sets

Wrong:

```python
items = {[1, 2], [3, 4]}
```

Lists are unhashable.

Use tuples:

```python
items = {(1, 2), (3, 4)}
```

---

## Mistake 4: Using Set When Duplicates Matter

If duplicates are meaningful, do not use a set.

Example:

```python
orders = ["paid", "paid", "failed"]
```

Using:

```python
set(orders)
```

returns:

```python
{"paid", "failed"}
```

You lose counts.

Use `collections.Counter` instead.

---

# Senior Deep Dive: Hash Collisions

Sets rely on hashing.

Two different objects can theoretically produce the same hash bucket.

This is called a collision.

Python handles collisions internally.

Average lookup remains O(1), but in pathological cases performance can degrade.

In interviews, the expected answer is:

```text
Set lookup is O(1) average case, not guaranteed O(1) in every possible case.
```

That phrase matters.

---

# Senior Deep Dive: Why Mutable Objects Cannot Be Set Elements

Suppose lists were allowed in sets:

```python
items = set()

x = [1, 2]

items.add(x)
```

Then:

```python
x.append(3)
```

The hash would need to change.

The set would no longer know where the object belongs.

This would break lookup.

That is why mutable objects like lists, dictionaries, and sets are unhashable.

---

# Interview Questions And Answers

## Q1. What is a set?

A set is an unordered collection of unique hashable elements.

---

## Q2. Why are sets useful?

Sets are useful for:

* Fast membership checks
* Removing duplicates
* Tracking seen values
* Mathematical set operations

---

## Q3. What is the complexity of `x in my_set`?

Average case:

```text
O(1)
```

Worst case can degrade due to hash collisions, but average-case lookup is constant time.

---

## Q4. Difference between list and set?

A list is ordered and allows duplicates.

A set is unordered and stores unique values.

List membership is O(n).

Set membership is O(1) average.

---

## Q5. Can a set contain a list?

No.

Lists are mutable and unhashable.

Use a tuple instead:

```python
items = {(1, 2), (3, 4)}
```

---

## Q6. How do you remove duplicates from a list?

If order does not matter:

```python
unique = list(set(items))
```

If order matters:

```python
unique = list(dict.fromkeys(items))
```

Or:

```python
seen = set()
result = []

for item in items:
    if item not in seen:
        seen.add(item)
        result.append(item)
```

---

## Q7. What is the difference between remove and discard?

```python
remove()
```

raises an error if the item does not exist.

```python
discard()
```

does nothing if the item does not exist.

---

## Q8. What is frozenset?

A `frozenset` is an immutable set.

It can be used as a dictionary key or stored inside another set.

---

## Q9. When should you avoid sets?

Avoid sets when:

* Order matters
* Duplicates matter
* You need index access
* Items are unhashable
* You need key-value mapping

---

## Q10. How are sets used in AI systems?

Sets are useful for:

* Deduplicating chunks
* Tracking retrieved document IDs
* Permission filtering
* Avoiding repeated embedding calls
* Tracking processed files

---

# Senior-Level Questions And Answers

## Senior Q1. Explain how sets work internally.

Sets use hash tables.

When inserting an item, Python computes its hash and uses that to determine where it should be stored internally.

When checking membership, Python computes the hash again and jumps near the expected location.

This is why membership checks are O(1) on average.

---

## Senior Q2. Why is `set(list_of_items)` not always safe?

It removes duplicates, which may be incorrect if duplicate counts matter.

It also does not preserve order in the same way a list does.

If counts matter, use `Counter`.

If order matters, use `dict.fromkeys()` or a manual seen-set pattern.

---

## Senior Q3. How would you optimize permission checks for 1 million documents?

Do not do this:

```python
if doc_id in allowed_doc_ids_list:
    ...
```

if `allowed_doc_ids_list` is large.

Convert to set once:

```python
allowed_doc_ids = set(allowed_doc_ids_list)

if doc_id in allowed_doc_ids:
    ...
```

This changes repeated lookup from O(n) to O(1) average.

---

## Senior Q4. Why might a set use more memory than a list?

Sets use hash tables and require extra space to maintain fast lookup performance.

Lists are compact arrays of references.

Use sets when lookup speed or uniqueness matters.

Use lists when order and compactness matter.

---

## Senior Q5. How would you deduplicate RAG chunks safely?

Use a normalized version of the chunk as the key.

Example:

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

For stronger deduplication, use a hash of normalized text.

---

# Exercises

## Exercise 1 — Create A Set

Create a set of programming languages:

```text
Python
Java
Go
TypeScript
```

---

## Exercise 2 — Remove Duplicates

Given:

```python
nums = [1, 2, 2, 3, 3, 4]
```

Return unique values.

---

## Exercise 3 — Check Duplicates

Implement:

```python
def has_duplicates(nums):
    pass
```

Return `True` if a list contains duplicates.

---

## Exercise 4 — Common Elements

Given two lists, return common elements.

```python
a = [1, 2, 3]
b = [2, 3, 4]
```

Output:

```python
[2, 3]
```

---

## Exercise 5 — Missing IDs

Given expected IDs and actual IDs, return missing IDs.

---

## Exercise 6 — Unique Words

Given a sentence, return unique words.

---

## Exercise 7 — Deduplicate While Preserving Order

Implement:

```python
def dedupe_ordered(items):
    pass
```

---

## Exercise 8 — Permission Filter

Given:

```python
allowed_ids = ["doc1", "doc3"]
results = ["doc1", "doc2", "doc3", "doc4"]
```

Return only allowed results.

---

## Exercise 9 — Symmetric Difference

Given two sets, return items that exist in one set but not both.

---

## Exercise 10 — Use frozenset As Key

Create a dictionary where the key is a frozenset of permissions.

---

## Exercise 11 — Detect Repeated Character

Implement:

```python
def first_repeated_char(text):
    pass
```

Input:

```python
"abca"
```

Output:

```python
"a"
```

---

## Exercise 12 — Check If Two Lists Have No Common Items

Implement:

```python
def disjoint(a, b):
    pass
```

---

# Solutions

## Solution 1

```python
languages = {"Python", "Java", "Go", "TypeScript"}
```

---

## Solution 2

```python
nums = [1, 2, 2, 3, 3, 4]

unique = list(set(nums))
```

If order matters:

```python
unique = list(dict.fromkeys(nums))
```

---

## Solution 3

```python
def has_duplicates(nums):
    return len(nums) != len(set(nums))
```

Alternative:

```python
def has_duplicates(nums):
    seen = set()

    for num in nums:
        if num in seen:
            return True

        seen.add(num)

    return False
```

---

## Solution 4

```python
def common_elements(a, b):
    return list(set(a) & set(b))
```

---

## Solution 5

```python
def missing_ids(expected, actual):
    return set(expected) - set(actual)
```

---

## Solution 6

```python
def unique_words(sentence):
    words = sentence.lower().split()
    return set(words)
```

---

## Solution 7

```python
def dedupe_ordered(items):
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

## Solution 8

```python
def filter_allowed(allowed_ids, results):
    allowed = set(allowed_ids)

    return [
        result
        for result in results
        if result in allowed
    ]
```

---

## Solution 9

```python
def symmetric_difference(a, b):
    return set(a) ^ set(b)
```

---

## Solution 10

```python
permissions = frozenset(["read", "write"])

permission_cache = {
    permissions: "editor"
}
```

---

## Solution 11

```python
def first_repeated_char(text):
    seen = set()

    for char in text:
        if char in seen:
            return char

        seen.add(char)

    return None
```

---

## Solution 12

```python
def disjoint(a, b):
    return len(set(a) & set(b)) == 0
```

Alternative:

```python
def disjoint(a, b):
    return set(a).isdisjoint(set(b))
```

---

# Mock Interview

## Interviewer

What is a set?

## Strong Candidate Answer

A set is an unordered collection of unique hashable elements. It is useful for fast membership checks, deduplication, and set operations like union and intersection.

---

## Interviewer

Why is set lookup faster than list lookup?

## Strong Candidate Answer

A list checks elements sequentially, so membership is O(n). A set uses a hash table, so Python can compute the hash of the item and jump near the expected location. That gives O(1) average lookup.

---

## Interviewer

Can a set contain a list?

## Strong Candidate Answer

No. Lists are mutable and unhashable. Set elements must be hashable because their hash determines where they are stored internally.

---

## Interviewer

How would you remove duplicates while preserving order?

## Strong Candidate Answer

I would use a seen set and a result list:

```python
seen = set()
result = []

for item in items:
    if item not in seen:
        seen.add(item)
        result.append(item)
```

This keeps the first occurrence and skips duplicates.

---

## Interviewer

How would sets appear in a RAG system?

## Strong Candidate Answer

Sets are useful for deduplicating chunks, tracking retrieved document IDs, filtering documents by permissions, and avoiding repeated embedding calls.

---

# Revision Sheet

## Set Basics

```python
items = {1, 2, 3}
```

Empty set:

```python
items = set()
```

---

## Add

```python
items.add(4)
```

---

## Remove

```python
items.remove(4)
```

Raises error if missing.

---

## Discard

```python
items.discard(4)
```

No error if missing.

---

## Membership

```python
x in items
```

Average:

```text
O(1)
```

---

## Union

```python
a | b
```

---

## Intersection

```python
a & b
```

---

## Difference

```python
a - b
```

---

## Symmetric Difference

```python
a ^ b
```

---

## Deduplicate

```python
list(set(items))
```

Order not guaranteed.

Preserve order:

```python
list(dict.fromkeys(items))
```

---

## frozenset

```python
key = frozenset(["read", "write"])
```

Immutable and hashable.

---

# Cheat Sheet

| Task                 | Code               |    |
| -------------------- | ------------------ | -- |
| Empty set            | `set()`            |    |
| Add item             | `s.add(x)`         |    |
| Remove item          | `s.remove(x)`      |    |
| Safe remove          | `s.discard(x)`     |    |
| Membership           | `x in s`           |    |
| Union                | `a                 | b` |
| Intersection         | `a & b`            |    |
| Difference           | `a - b`            |    |
| Symmetric difference | `a ^ b`            |    |
| Immutable set        | `frozenset(items)` |    |

---

# Final Key Takeaways

Sets are essential for writing efficient Python.

Use sets when you need:

* Fast lookup
* Uniqueness
* Deduplication
* Tracking seen values
* Mathematical set operations

Avoid sets when you need:

* Order
* Duplicates
* Index access
* Mutable elements
* Key-value mapping

For interviews, remember:

```text
List membership: O(n)

Set membership: O(1) average
```

That one decision can turn an inefficient solution into an optimal one.
