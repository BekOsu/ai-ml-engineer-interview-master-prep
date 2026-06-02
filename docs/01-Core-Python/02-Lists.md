# Python Lists

> Dynamic arrays, memory layout, complexity analysis, and production usage.

---

# Why Lists Matter

Lists are the most frequently used data structure in Python.

You will encounter them everywhere:

* API responses
* Database query results
* Machine Learning datasets
* FastAPI services
* RAG pipelines
* Batch processing
* Data transformations

Most Python interview questions involve lists directly or indirectly.

Examples:

* Two Sum
* Sliding Window
* Merge Intervals
* Rotate Array
* Prefix Sum
* Top K Elements

Understanding lists deeply is one of the highest ROI investments you can make.

---

# Learning Objectives

By the end of this chapter you should be able to:

* Explain how Python lists work internally
* Explain why append() is fast
* Explain why insert(0, x) is slow
* Analyze list complexity
* Use list comprehensions effectively
* Choose lists vs sets vs dictionaries
* Optimize list-heavy code
* Solve common interview problems

---

# What Is A List?

A Python list is:

* Ordered
* Mutable
* Dynamic in size
* Heterogeneous

Example:

```python
data = [1, "hello", 3.14, True]
```

Python allows different object types in the same list.

---

# Internal Implementation

Many developers imagine:

```python
nums = [10, 20, 30]
```

as:

```text
nums
│
▼
[10,20,30]
```

This is not how Python stores lists.

Internally:

```text
nums
│
▼

+-----+-----+-----+
|  *  |  *  |  *  |
+-----+-----+-----+
   │      │      │
   ▼      ▼      ▼

  10     20     30
```

The list stores references.

Not the actual objects.

This is extremely important.

---

# Why References Matter

Example:

```python
a = [1000]

b = a

b.append(2000)

print(a)
```

Output:

```python
[1000, 2000]
```

Because:

```text
a ─┐
   │
   ▼
 [1000]
   ▲
   │
b ─┘
```

Both variables reference the same list object.

---

# Dynamic Arrays

Python lists are implemented as dynamic arrays.

Unlike traditional arrays:

```text
Fixed Size
```

Lists can grow.

Example:

```python
nums = []

nums.append(1)
nums.append(2)
nums.append(3)
```

No manual resizing required.

Python handles growth automatically.

---

# Memory Growth Strategy

Interview favorite.

Question:

Why is:

```python
nums.append(x)
```

usually O(1)?

Shouldn't resizing be expensive?

Answer:

Python allocates extra memory ahead of time.

Example:

You store:

```text
Capacity = 4

+---+---+---+---+
| 1 | 2 |   |   |
+---+---+---+---+
```

Append:

```python
nums.append(3)
```

No resize required.

Still:

```text
Capacity = 4

+---+---+---+---+
| 1 | 2 | 3 |   |
+---+---+---+---+
```

Very fast.

---

# What Happens When Capacity Is Full?

Before:

```text
Capacity = 4

+---+---+---+---+
| 1 | 2 | 3 | 4 |
+---+---+---+---+
```

Append:

```python
nums.append(5)
```

Python:

1. Allocates larger memory block
2. Copies references
3. Adds new element

Conceptually:

```text
Old

+---+---+---+---+
| 1 | 2 | 3 | 4 |
+---+---+---+---+

↓

New

+---+---+---+---+---+---+---+---+
| 1 | 2 | 3 | 4 | 5 |   |   |   |
+---+---+---+---+---+---+---+---+
```

This resize operation is O(n).

---

# Amortized O(1)

Senior interview topic.

Although some appends require resizing:

```text
Most appends do not.
```

Therefore:

```python
append()
```

is considered:

```text
O(1) Amortized
```

Not:

```text
O(n)
```

This distinction is important.

---

# Complexity Table

| Operation       | Complexity     |
| --------------- | -------------- |
| Access by Index | O(1)           |
| Update by Index | O(1)           |
| Append          | O(1) Amortized |
| Pop End         | O(1)           |
| Insert Front    | O(n)           |
| Remove Front    | O(n)           |
| Search          | O(n)           |
| Membership (in) | O(n)           |
| Copy            | O(n)           |
| Sort            | O(n log n)     |

Memorize this table.

Interviewers love it.

---

# Core Operations

## Append

```python
nums = [1, 2]

nums.append(3)
```

Result:

```python
[1, 2, 3]
```

Complexity:

```text
O(1) Amortized
```

---

## Extend

```python
a = [1, 2]

a.extend([3, 4])
```

Result:

```python
[1, 2, 3, 4]
```

---

## Insert

```python
nums = [1, 2, 3]

nums.insert(0, 100)
```

Result:

```python
[100, 1, 2, 3]
```

Complexity:

```text
O(n)
```

Reason:

Every element must shift.

---

## Remove

```python
nums.remove(3)
```

Removes first matching value.

Complexity:

```text
O(n)
```

Search required.

---

## Pop

```python
nums.pop()
```

Removes last item.

Complexity:

```text
O(1)
```

---

## Pop Front

```python
nums.pop(0)
```

Complexity:

```text
O(n)
```

All remaining elements shift.

---

## Membership

```python
if 10 in nums:
    pass
```

Complexity:

```text
O(n)
```

Linear search.

---

# Common Interview Question

Why is this slow?

```python
nums = []

for i in range(1_000_000):
    nums.insert(0, i)
```

Answer:

Every insert shifts all elements.

Complexity:

```text
O(n²)
```

Terrible performance.

---

# Better Solution

```python
nums = []

for i in range(1_000_000):
    nums.append(i)
```

Complexity:

```text
O(n)
```

Huge improvement.

---

# Production Engineering Note

Bad:

```python
result = []

for item in items:
    result = result + [item]
```

Each iteration creates a new list.

Complexity:

```text
O(n²)
```

---

Good:

```python
result = []

for item in items:
    result.append(item)
```

Complexity:

```text
O(n)
```

This type of optimization appears frequently in backend services and ETL pipelines.

---

# AI Engineering Example

Imagine:

```python
embeddings = []
```

Processing:

```python
for chunk in chunks:
    embeddings.append(
        embed(chunk)
    )
```

This is efficient because append is amortized O(1).

If you're processing:

```text
100,000 chunks
```

understanding list behavior directly impacts system performance.

---

# Key Takeaways (Part A)

* Python lists are dynamic arrays
* Lists store references, not objects
* append() is O(1) amortized
* insert(0, x) is O(n)
* pop() is O(1)
* pop(0) is O(n)
* Membership checks are O(n)
* Python over-allocates memory to reduce resizing
* Lists are one of the most important interview topics
* Lists are heavily used in AI and backend systems

# Slicing

Slicing is one of Python's most powerful features.

Syntax:

```python
list[start:end:step]
```

General form:

```python
arr[start:end]
```

Meaning:

```text
Include start
Exclude end
```

Example:

```python
nums = [10, 20, 30, 40, 50]

print(nums[1:4])
```

Output:

```python
[20, 30, 40]
```

Visualization:

```text
Index:

0   1   2   3   4
│   │   │   │   │
10 20 30 40 50

nums[1:4]
      │------│

Result:
20 30 40
```

---

# Negative Indexing

Python supports negative indexes.

Example:

```python
nums = [10, 20, 30, 40, 50]

print(nums[-1])
```

Output:

```python
50
```

Visualization:

```text
 Positive:
 0  1  2  3  4

 Negative:
-5 -4 -3 -2 -1
```

---

# Common Slicing Patterns

## First N Elements

```python
nums[:3]
```

Result:

```python
[10, 20, 30]
```

---

## Last N Elements

```python
nums[-3:]
```

Result:

```python
[30, 40, 50]
```

---

## Copy List

```python
copy_nums = nums[:]
```

Creates a shallow copy.

Complexity:

```text
O(n)
```

---

## Reverse List

```python
nums[::-1]
```

Example:

```python
[1, 2, 3]
```

Result:

```python
[3, 2, 1]
```

---

# Interview Question

What is complexity of:

```python
nums[:]
```

Answer:

```text
O(n)
```

Because Python creates a new list.

Many candidates incorrectly answer O(1).

---

# List Comprehensions

One of the most Pythonic features.

Instead of:

```python
squares = []

for x in nums:
    squares.append(x * x)
```

Use:

```python
squares = [x * x for x in nums]
```

Cleaner.

Usually faster.

---

# Anatomy of List Comprehension

```python
[new_value for item in iterable]
```

Example:

```python
nums = [1, 2, 3]

squares = [x * x for x in nums]
```

Result:

```python
[1, 4, 9]
```

---

# Filtering

Example:

```python
nums = [1, 2, 3, 4, 5, 6]

evens = [x for x in nums if x % 2 == 0]
```

Result:

```python
[2, 4, 6]
```

---

# Conditional Transformation

Example:

```python
labels = [
    "even" if x % 2 == 0 else "odd"
    for x in nums
]
```

Result:

```python
['odd', 'even', 'odd', 'even']
```

---

# Nested Comprehensions

Example:

```python
matrix = [
    [1, 2],
    [3, 4]
]

flat = [
    item
    for row in matrix
    for item in row
]
```

Result:

```python
[1, 2, 3, 4]
```

---

# Interview Question

Which is better?

```python
for-loop
```

or

```python
list comprehension
```

Answer:

Use list comprehension when:

* Simple transformation
* Simple filtering

Avoid when:

* Complex logic
* Multiple nested conditions
* Reduced readability

Readability wins.

---

# Sorting

Another favorite interview topic.

---

# sorted()

Returns a new list.

```python
nums = [3, 1, 2]

result = sorted(nums)
```

Result:

```python
[1, 2, 3]
```

Original list unchanged.

---

# list.sort()

Modifies list in place.

```python
nums.sort()
```

Result:

```python
[1, 2, 3]
```

Original list changed.

---

# Interview Question

Difference?

```python
sorted()
```

vs

```python
list.sort()
```

Answer:

```python
sorted()
```

returns new list.

```python
list.sort()
```

modifies existing list.

---

# Sorting Descending

```python
nums.sort(reverse=True)
```

Result:

```python
[9, 7, 5, 1]
```

---

# Sorting Strings

```python
names = [
    "Charlie",
    "Alice",
    "Bob"
]

names.sort()
```

Result:

```python
['Alice', 'Bob', 'Charlie']
```

---

# Custom Sorting

Very common.

Example:

```python
users = [
    ("Ali", 25),
    ("Sara", 20),
    ("John", 30)
]

users.sort(
    key=lambda user: user[1]
)
```

Result:

```python
[
 ('Sara',20),
 ('Ali',25),
 ('John',30)
]
```

---

# AI Engineering Example

Sort retrieval results by score:

```python
results.sort(
    key=lambda item: item["score"],
    reverse=True
)
```

Common in:

* RAG systems
* Search engines
* Recommendation systems

---

# Timsort

Python uses:

```text
Timsort
```

Characteristics:

* Stable
* Very efficient
* Optimized for real-world data

Complexity:

```text
Best:
O(n)

Average:
O(n log n)

Worst:
O(n log n)
```

Interviewers occasionally ask this.

---

# Common List Mistakes

## Mistake #1

Shared Nested Lists

Bad:

```python
matrix = [[0] * 3] * 3
```

Looks like:

```text
3 independent rows
```

Actually:

```text
3 references
to same row
```

Problem:

```python
matrix[0][0] = 1
```

Result:

```python
[
 [1,0,0],
 [1,0,0],
 [1,0,0]
]
```

---

# Correct Version

```python
matrix = [
    [0] * 3
    for _ in range(3)
]
```

---

# Mistake #2

Repeated Concatenation

Bad:

```python
result = []

for x in nums:
    result = result + [x]
```

Complexity:

```text
O(n²)
```

---

# Correct

```python
result.append(x)
```

Complexity:

```text
O(n)
```

---

# Mistake #3

Using List For Membership Checks

Bad:

```python
allowed = [1,2,3,4,5]

if user_id in allowed:
    pass
```

Complexity:

```text
O(n)
```

---

# Better

```python
allowed = {1,2,3,4,5}
```

Complexity:

```text
O(1)
```

---

# Production Engineering Notes

When lists become large:

```text
Millions of rows
Millions of embeddings
Millions of events
```

Performance matters.

Questions:

* Should this remain a list?
* Should it become NumPy?
* Should it become a set?
* Should it move to Redis?
* Should it be streamed?

Senior engineers ask these questions constantly.

---

# AI Engineering Example

Embedding Pipeline

```python
embeddings = []

for chunk in chunks:
    embeddings.append(
        model.embed(chunk)
    )
```

Good for:

```text
Thousands
```

Potentially problematic for:

```text
Millions
```

At scale you may need:

* NumPy arrays
* Vector databases
* Streaming pipelines

instead of one giant Python list.

---

# Senior Deep Dive

Question:

Why does NumPy often outperform Python lists?

Answer:

Python list:

```text
Array of references
```

NumPy:

```text
Contiguous memory
Fixed type
Vectorized operations
```

Result:

* Better CPU cache usage
* Less memory
* Faster computation

This becomes important in ML systems.

---

# Key Takeaways (Part B)

* Slicing creates new lists
* Slicing is usually O(n)
* List comprehensions are Pythonic and efficient
* sorted() returns new list
* sort() modifies existing list
* Python uses Timsort
* Avoid repeated concatenation
* Avoid shared nested lists
* Use sets for membership checks
* Lists are great, but not always the best structure

# Interview Questions

---

# Junior Level Questions

## Q1. What is a Python List?

### Expected Answer

A list is:

* Ordered
* Mutable
* Dynamic in size
* Can store mixed data types

Example:

```python
data = [1, "hello", 3.14]
```

---

## Q2. How do you create a list?

```python
nums = [1, 2, 3]

empty = []
```

---

## Q3. How do you access an element?

```python
nums[0]
```

Complexity:

```text
O(1)
```

---

## Q4. Difference between List and Tuple?

### List

```python
nums = [1, 2, 3]
```

Mutable.

### Tuple

```python
nums = (1, 2, 3)
```

Immutable.

---

## Q5. Can a list contain multiple data types?

Yes.

```python
data = [
    1,
    "hello",
    True,
    3.14
]
```

Although generally not recommended in production code.

---

# Mid-Level Questions

---

## Q6. Why is append() O(1)?

### Expected Answer

Lists are dynamic arrays.

Python overallocates memory.

Most appends do not require resizing.

Therefore:

```python
append()
```

is:

```text
O(1) Amortized
```

---

## Q7. Why is insert(0, x) O(n)?

### Expected Answer

Every element must shift one position.

Example:

Before:

```text
[1,2,3]
```

After:

```text
[100,1,2,3]
```

All existing elements move.

Complexity:

```text
O(n)
```

---

## Q8. Why is list membership O(n)?

Example:

```python
10 in nums
```

Python scans elements sequentially.

Worst case:

```text
O(n)
```

---

## Q9. What is slicing complexity?

Example:

```python
nums[:]
```

Creates a new list.

Complexity:

```text
O(n)
```

---

## Q10. Difference Between:

```python
sorted(nums)
```

and:

```python
nums.sort()
```

### Answer

```python
sorted()
```

Returns a new list.

```python
sort()
```

Modifies original list.

---

# Senior-Level Questions

---

## Q11. Explain how Python lists work internally.

### Strong Answer

Python lists are dynamic arrays.

Internally they store references:

```text
+-----+-----+-----+
|  *  |  *  |  *  |
+-----+-----+-----+
```

Each slot contains a pointer to an object.

Lists grow by allocating larger memory blocks and copying references.

---

## Q12. Why is append() not truly O(1)?

### Strong Answer

Sometimes append triggers resizing.

Example:

```text
Capacity Full
```

Python:

1. Allocates new memory
2. Copies references
3. Adds new element

That specific operation is:

```text
O(n)
```

However most appends are O(1).

Therefore:

```text
Amortized O(1)
```

---

## Q13. When would you avoid lists?

### Strong Answer

Avoid lists when:

#### Fast Membership Needed

Use:

```python
set
```

#### Key-Value Lookup Needed

Use:

```python
dict
```

#### Numerical Computation Needed

Use:

```python
numpy.ndarray
```

#### Streaming Needed

Use:

```python
generator
```

---

## Q14. Why are NumPy arrays faster?

### Strong Answer

Python list:

```text
Array of references
```

NumPy:

```text
Contiguous memory
Same data type
Vectorized operations
```

Benefits:

* Better CPU cache usage
* Less memory
* Faster execution

---

## Q15. Explain a production bug caused by lists.

### Strong Answer

Shared nested list bug:

Bad:

```python
matrix = [[0] * 3] * 3
```

All rows reference same object.

Modification affects all rows.

Fix:

```python
matrix = [
    [0] * 3
    for _ in range(3)
]
```

---

# AI Engineer Questions

---

## Q16. Why is storing millions of embeddings in a Python list problematic?

### Strong Answer

Problems:

* Large RAM usage
* Linear membership search
* Poor persistence
* Difficult horizontal scaling

Better:

* NumPy
* FAISS
* Vector DB
* Milvus
* Weaviate
* pgvector

---

## Q17. How would you batch embeddings efficiently?

Bad:

```python
for doc in docs:
    embed(doc)
```

Better:

```python
embed_batch(docs)
```

Reduces:

* Network overhead
* API calls
* Serialization cost

---

## Q18. Why might list comprehensions be useful in data pipelines?

Example:

```python
cleaned = [
    x.strip()
    for x in texts
]
```

Advantages:

* Cleaner
* Faster
* Easier to read

---

# Senior Deep Dive

---

## Question

Which is faster?

```python
if x in my_list
```

or

```python
if x in my_set
```

### Answer

List:

```text
O(n)
```

Set:

```text
O(1)
```

For:

```text
1,000,000 records
```

difference is massive.

---

## Question

Would you store 50 million records in a Python list?

### Answer

Usually no.

Reasons:

* Memory overhead
* Scalability
* Serialization cost

Consider:

* NumPy
* Database
* Data lake
* Object storage

---

## Question

How would you profile list-heavy code?

### Answer

Tools:

```python
cProfile
```

```python
timeit
```

```python
tracemalloc
```

Investigate:

* Copies
* Slicing
* Membership checks
* Nested loops

---

# Common Interview Traps

---

## Trap 1

Question:

```python
nums[:]
```

Complexity?

Many candidates answer:

```text
O(1)
```

Correct:

```text
O(n)
```

---

## Trap 2

Question:

```python
nums[::-1]
```

Does it modify original list?

Answer:

No.

Creates a new list.

---

## Trap 3

Question:

```python
a = [1,2]
b = a[:]
```

Shared object?

Answer:

No.

New outer list created.

---

## Trap 4

Question:

```python
matrix = [[0]*3]*3
```

Independent rows?

Answer:

No.

Shared references.

---

# Interview Preparation Checklist

Before moving on, you should be able to explain:

* Dynamic arrays
* References
* Over-allocation
* Amortized O(1)
* Slicing complexity
* List comprehension usage
* Timsort
* List vs Tuple
* List vs Set
* List vs Dict
* List vs NumPy

without notes.

---

# Key Takeaways

Senior engineers are expected to understand:

* Internal implementation
* Memory behavior
* Complexity analysis
* Tradeoffs

Not just syntax.

The difference between:

```python
append()
```

and:

```python
insert(0)
```

can determine whether a system runs in seconds or hours.

Master these concepts before moving to Tuples.

# Exercises

## Easy

### Exercise 1 — Find Maximum

Implement:

```python
def find_max(nums):
    pass
```

Input:

```python
[3, 7, 2, 9, 1]
```

Output:

```python
9
```

---

### Exercise 2 — Find Minimum

Implement:

```python
def find_min(nums):
    pass
```

---

### Exercise 3 — Sum All Elements

Implement:

```python
def list_sum(nums):
    pass
```

---

### Exercise 4 — Reverse List

Without using:

```python
reverse()
[::-1]
```

Implement:

```python
def reverse_list(nums):
    pass
```

---

### Exercise 5 — Count Even Numbers

Implement:

```python
def count_even(nums):
    pass
```

---

## Medium

### Exercise 6 — Remove Duplicates

Input:

```python
[1,1,2,2,3,4,4]
```

Output:

```python
[1,2,3,4]
```

---

### Exercise 7 — Rotate Array

Rotate:

```python
[1,2,3,4,5]
```

by:

```python
k = 2
```

Result:

```python
[4,5,1,2,3]
```

---

### Exercise 8 — Two Sum

Input:

```python
nums = [2,7,11,15]
target = 9
```

Output:

```python
[0,1]
```

---

### Exercise 9 — Merge Sorted Lists

Input:

```python
[1,3,5]
[2,4,6]
```

Output:

```python
[1,2,3,4,5,6]
```

---

### Exercise 10 — Find Missing Number

Input:

```python
[1,2,3,5]
```

Output:

```python
4
```

---

## Hard

### Exercise 11 — Product Except Self

Input:

```python
[1,2,3,4]
```

Output:

```python
[24,12,8,6]
```

---

### Exercise 12 — Maximum Subarray

Implement Kadane's Algorithm.

---

### Exercise 13 — Sliding Window Maximum

Given:

```python
nums = [1,3,-1,-3,5,3,6,7]
```

Window size:

```python
k = 3
```

Find maximum in each window.

---

### Exercise 14 — Merge Intervals

Input:

```python
[[1,3],[2,6],[8,10],[15,18]]
```

Output:

```python
[[1,6],[8,10],[15,18]]
```

---

### Exercise 15 — Trapping Rain Water

Classic hard interview question.

---

# Solutions

## Solution 1

```python
def find_max(nums):
    maximum = nums[0]

    for num in nums:
        if num > maximum:
            maximum = num

    return maximum
```

Complexity:

```text
O(n)
```

---

## Solution 2

```python
def find_min(nums):
    minimum = nums[0]

    for num in nums:
        if num < minimum:
            minimum = num

    return minimum
```

---

## Solution 3

```python
def list_sum(nums):
    total = 0

    for num in nums:
        total += num

    return total
```

---

## Solution 4

```python
def reverse_list(nums):

    left = 0
    right = len(nums) - 1

    while left < right:

        nums[left], nums[right] = (
            nums[right],
            nums[left]
        )

        left += 1
        right -= 1

    return nums
```

---

## Solution 5

```python
def count_even(nums):

    count = 0

    for num in nums:
        if num % 2 == 0:
            count += 1

    return count
```

---

## Solution 6

```python
def remove_duplicates(nums):

    result = []

    for num in nums:
        if num not in result:
            result.append(num)

    return result
```

Optimized:

```python
list(dict.fromkeys(nums))
```

---

## Solution 7

```python
def rotate(nums, k):

    k %= len(nums)

    return nums[-k:] + nums[:-k]
```

---

## Solution 8

```python
def two_sum(nums, target):

    seen = {}

    for i, num in enumerate(nums):

        diff = target - num

        if diff in seen:
            return [seen[diff], i]

        seen[num] = i
```

Complexity:

```text
O(n)
```

---

## Solution 9

```python
def merge_sorted(a, b):

    result = []

    i = 0
    j = 0

    while i < len(a) and j < len(b):

        if a[i] < b[j]:
            result.append(a[i])
            i += 1
        else:
            result.append(b[j])
            j += 1

    result.extend(a[i:])
    result.extend(b[j:])

    return result
```

---

## Solution 10

```python
def missing_number(nums):

    expected = sum(
        range(
            1,
            len(nums)+2
        )
    )

    return expected - sum(nums)
```

---

## Solution 11

```python
def product_except_self(nums):

    n = len(nums)

    left = [1] * n
    right = [1] * n

    for i in range(1, n):
        left[i] = left[i-1] * nums[i-1]

    for i in range(n-2, -1, -1):
        right[i] = right[i+1] * nums[i+1]

    return [
        left[i] * right[i]
        for i in range(n)
    ]
```

---

## Solution 12

Kadane's Algorithm

```python
def max_subarray(nums):

    current = nums[0]
    best = nums[0]

    for num in nums[1:]:

        current = max(
            num,
            current + num
        )

        best = max(best, current)

    return best
```

---

## Solutions 13–15

Leave for self-practice first.

Return later after attempting.

This improves retention significantly.

---

# Mock Interview

## Question 1

Why is:

```python
append()
```

O(1)

but:

```python
insert(0, x)
```

O(n)?

### Expected Answer

Append usually writes into preallocated memory.

Insert at front shifts all elements.

---

## Question 2

Explain:

```python
nums[::-1]
```

### Expected Answer

Creates a new reversed list.

Complexity:

```text
O(n)
```

---

## Question 3

Difference between:

```python
sort()
```

and:

```python
sorted()
```

### Expected Answer

sort():

* In place

sorted():

* New list

---

## Question 4

Would you store:

```text
50 million embeddings
```

in a Python list?

### Strong Answer

Usually no.

Prefer:

* NumPy
* Vector DB
* FAISS
* Milvus
* Weaviate

depending on requirements.

---

## Question 5

Why is this slow?

```python
result = []

for item in data:
    result = result + [item]
```

### Strong Answer

Creates a new list every iteration.

Complexity:

```text
O(n²)
```

---

# Revision Sheet

## Complexity

| Operation    | Complexity     |
| ------------ | -------------- |
| Access       | O(1)           |
| Update       | O(1)           |
| Append       | O(1) Amortized |
| Pop End      | O(1)           |
| Search       | O(n)           |
| Membership   | O(n)           |
| Insert Front | O(n)           |
| Remove Front | O(n)           |
| Sort         | O(n log n)     |

---

## Remember

Lists are:

```text
Ordered
Mutable
Dynamic
Reference-based
```

---

## Common Mistakes

Avoid:

```python
[[0] * 3] * 3
```

Avoid:

```python
result = result + [x]
```

Avoid:

```python
if x in huge_list
```

for frequent lookups.

Use:

```python
set
```

instead.

---

# Cheat Sheet

## Copy

```python
nums[:]
```

---

## Reverse

```python
nums[::-1]
```

---

## Sort

```python
nums.sort()
```

---

## New Sorted List

```python
sorted(nums)
```

---

## Last Element

```python
nums[-1]
```

---

## Last N Elements

```python
nums[-n:]
```

---

## First N Elements

```python
nums[:n]
```

---

## Flatten

```python
[
    item
    for row in matrix
    for item in row
]
```

---

# Final Key Takeaways

Lists are the most important Python data structure.

Master:

* Dynamic arrays
* References
* Complexity analysis
* Slicing
* Sorting
* List comprehensions

before moving to:

```text
03-Tuples.md
```

Everything else in Python builds on these foundations.
