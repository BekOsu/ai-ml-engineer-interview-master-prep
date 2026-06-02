# Core Python Mock Interview

> A realistic mock interview for Senior Software Engineer and AI Engineer roles, focused on Core Python fundamentals, debugging, performance, and production reasoning.

---

# How To Use This File

This file simulates a real interview.

Do not read the strong answers immediately.

Use this flow:

```text
1. Read the interviewer question.
2. Answer out loud.
3. Write your code.
4. Explain complexity.
5. Compare with the strong answer.
6. Repeat without looking.
```

The goal is to practice:

* speaking clearly
* writing clean Python
* explaining tradeoffs
* debugging under pressure
* connecting Python fundamentals to production AI systems

---

# Interview Format

This mock interview has 5 rounds:

```text
Round 1 — Python Memory Model
Round 2 — Lists, Sets, Dictionaries
Round 3 — Functions And Code Quality
Round 4 — Debugging And Performance
Round 5 — AI Engineering Python Scenario
```

Expected duration:

```text
60–90 minutes
```

---

# Evaluation Criteria

A strong candidate should demonstrate:

* clear reasoning
* clean code
* correct complexity analysis
* awareness of edge cases
* good data structure choices
* production mindset
* ability to debug calmly
* ability to connect Python to real AI engineering systems

---

# Round 1 — Python Memory Model

---

## Question 1

### Interviewer

What will this code print?

```python
a = [1, 2, 3]
b = a

b.append(4)

print(a)
print(b)
```

---

## Candidate Task

Explain:

* output
* why it happens
* what is stored in `a`
* what is stored in `b`

---

## Strong Answer

Output:

```python
[1, 2, 3, 4]
[1, 2, 3, 4]
```

`a` and `b` both reference the same list object.

The assignment:

```python
b = a
```

does not copy the list.

It copies the reference.

So when we call:

```python
b.append(4)
```

we mutate the shared list object.

Both variables now observe the modified list.

Conceptually:

```text
a ─┐
   ├──► [1, 2, 3, 4]
b ─┘
```

---

## Follow-Up Question

How would you make `b` independent from `a`?

---

## Strong Answer

Use a shallow copy if the list is flat:

```python
b = a.copy()
```

or:

```python
b = a[:]
```

For nested mutable objects, use:

```python
import copy

b = copy.deepcopy(a)
```

---

## Question 2

### Interviewer

What is the difference between `==` and `is`?

---

## Strong Answer

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

The two lists contain the same values, so `a == b` is `True`.

But they are different objects in memory, so `a is b` is `False`.

---

## Question 3

### Interviewer

What is wrong with this function?

```python
def add_item(item, items=[]):
    items.append(item)
    return items
```

---

## Strong Answer

The default list is created once when the function is defined.

That same list is reused across calls.

Example:

```python
print(add_item("a"))
print(add_item("b"))
print(add_item("c"))
```

Output:

```python
['a']
['a', 'b']
['a', 'b', 'c']
```

Correct version:

```python
def add_item(item, items=None):
    if items is None:
        items = []

    items.append(item)
    return items
```

This creates a new list for each call when no list is provided.

---

# Round 2 — Lists, Sets, Dictionaries

---

## Question 4 — Coding

### Interviewer

Write a function that removes duplicates from a list while preserving order.

```python
def remove_duplicates_ordered(items: list[int]) -> list[int]:
    pass
```

Example:

```python
remove_duplicates_ordered([1, 2, 2, 3, 1, 4])
```

Expected output:

```python
[1, 2, 3, 4]
```

---

## Strong Solution

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

---

## Explanation

We use a set for fast membership checks.

We use a list to preserve order.

Using only:

```python
list(set(items))
```

would remove duplicates but would not reliably preserve the original order.

---

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

## Question 5 — Coding

### Interviewer

Implement Two Sum.

```python
def two_sum(nums: list[int], target: int) -> list[int]:
    pass
```

Example:

```python
two_sum([2, 7, 11, 15], 9)
```

Expected output:

```python
[0, 1]
```

---

## Strong Solution

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

---

## Explanation

For each number, we compute the needed complement:

```python
diff = target - num
```

If that complement was already seen, we found the answer.

The dictionary maps:

```text
number -> index
```

---

## Complexity

```text
Time:  O(n)
Space: O(n)
```

---

## Follow-Up Question

What is the brute-force complexity?

---

## Strong Answer

Brute force checks every pair:

```text
Time:  O(n²)
Space: O(1)
```

The dictionary solution improves time to O(n), but uses O(n) extra memory.

This is a time-space tradeoff.

---

## Question 6

### Interviewer

When would you use a set instead of a list?

---

## Strong Answer

Use a set when you need:

* fast membership checks
* uniqueness
* deduplication
* seen/visited tracking

Example:

```python
allowed_ids = set(allowed_ids)

if user_id in allowed_ids:
    ...
```

List membership is O(n).

Set membership is O(1) average.

Use a list when order, duplicates, or index access matters.

---

## Question 7

### Interviewer

When would you use a dictionary instead of a list?

---

## Strong Answer

Use a dictionary when you need key-value lookup.

Example:

```python
users_by_id = {
    "u1": {"id": "u1", "name": "Ali"},
    "u2": {"id": "u2", "name": "Sara"},
}
```

Now:

```python
users_by_id["u1"]
```

is O(1) average.

If we stored users in a list, we would need to scan the list, which is O(n).

---

# Round 3 — Functions And Code Quality

---

## Question 8

### Interviewer

What makes a function good?

---

## Strong Answer

A good function should:

* do one thing
* have a clear name
* have clear inputs
* return clear outputs
* avoid hidden side effects
* be easy to test
* fail clearly
* avoid unnecessary global state

Example of a vague function:

```python
def process(data):
    ...
```

Better:

```python
def normalize_invoice_payload(payload):
    ...
```

Specific names make code easier to understand.

---

## Question 9 — Coding

### Interviewer

Write a function that normalizes text for a RAG preprocessing pipeline.

Requirements:

* strip leading/trailing spaces
* lowercase
* collapse repeated spaces

```python
def normalize_text(text: str) -> str:
    pass
```

---

## Strong Solution

```python
def normalize_text(text: str) -> str:
    return " ".join(
        text.strip().lower().split()
    )
```

---

## Example

```python
normalize_text("  Python   FUNCTIONS ")
```

Output:

```python
"python functions"
```

---

## Complexity

Let `n` be text length.

```text
Time:  O(n)
Space: O(n)
```

---

## Follow-Up Question

How would you handle `None` input?

---

## Strong Answer

It depends on requirements.

Permissive version:

```python
def normalize_text(text: str | None) -> str:
    if text is None:
        return ""

    return " ".join(text.strip().lower().split())
```

Strict version:

```python
def normalize_text(text: str) -> str:
    if text is None:
        raise ValueError("text must not be None")

    return " ".join(text.strip().lower().split())
```

In production, I would choose based on whether `None` is valid business input.

---

## Question 10

### Interviewer

What is a closure?

---

## Strong Answer

A closure is a function that remembers variables from its enclosing scope.

Example:

```python
def make_prefixer(prefix: str):
    def add_prefix(text: str) -> str:
        return f"{prefix}{text}"

    return add_prefix
```

Usage:

```python
error = make_prefixer("ERROR: ")
print(error("Invalid input"))
```

Output:

```python
ERROR: Invalid input
```

The inner function remembers `prefix` even after the outer function finishes.

---

## Question 11

### Interviewer

How would you make this function easier to test?

```python
def get_embedding(text):
    return openai_client.embed(text)
```

---

## Strong Answer

Inject the dependency instead of using a global client.

```python
def get_embedding(text, embedding_client):
    return embedding_client.embed(text)
```

Now in tests, I can pass a fake client.

Example:

```python
class FakeEmbeddingClient:
    def embed(self, text):
        return [0.1, 0.2, 0.3]
```

This makes the function easier to test and avoids hidden dependencies.

---

# Round 4 — Debugging And Performance

---

## Question 12 — Debugging

### Interviewer

This code fails sometimes. Why?

```python
def get_source(document):
    return document["metadata"]["source"]
```

Example input:

```python
document = {
    "id": "doc_1",
    "text": "Python debugging"
}
```

---

## Strong Answer

The code assumes that `metadata` exists.

For the given input, `document["metadata"]` raises:

```text
KeyError: 'metadata'
```

A safer version:

```python
def get_source(document):
    metadata = document.get("metadata") or {}
    return metadata.get("source", "unknown")
```

If metadata is required, I would fail clearly:

```python
def get_source(document):
    if "metadata" not in document:
        raise ValueError(f"Document missing metadata: {document.get('id')}")

    return document["metadata"].get("source", "unknown")
```

The right approach depends on whether metadata is optional or required.

---

## Question 13 — Debugging

### Interviewer

What is wrong here?

```python
matrix = [[0] * 3] * 3

matrix[0][0] = 1

print(matrix)
```

---

## Strong Answer

All rows reference the same inner list.

Output:

```python
[
    [1, 0, 0],
    [1, 0, 0],
    [1, 0, 0],
]
```

Correct version:

```python
matrix = [
    [0] * 3
    for _ in range(3)
]
```

This creates independent rows.

---

## Question 14 — Performance

### Interviewer

Why is this slow?

```python
result = []

for item in items:
    result = result + [item]
```

---

## Strong Answer

Each iteration creates a new list and copies existing elements.

This can become O(n²).

Better:

```python
result = []

for item in items:
    result.append(item)
```

or:

```python
result = [item for item in items]
```

The improved version is O(n).

---

## Question 15 — Performance

### Interviewer

How would you optimize this?

```python
def filter_allowed_documents(documents, allowed_ids):
    result = []

    for doc in documents:
        if doc["id"] in allowed_ids:
            result.append(doc)

    return result
```

Assume `allowed_ids` is a large list.

---

## Strong Solution

```python
def filter_allowed_documents(documents, allowed_ids):
    allowed = set(allowed_ids)

    result = []

    for doc in documents:
        if doc["id"] in allowed:
            result.append(doc)

    return result
```

---

## Explanation

Original:

```text
documents = n
allowed_ids = m

Time: O(n * m)
```

because each membership check scans the list.

Optimized:

```text
Convert allowed_ids to set: O(m)
Check documents: O(n)

Total: O(n + m)
```

Space:

```text
O(m)
```

This is a classic time-space tradeoff.

---

# Round 5 — AI Engineering Python Scenario

---

## Scenario

### Interviewer

You are building a small preprocessing step for a RAG pipeline.

You receive text chunks like this:

```python
chunks = [
    " Python   Functions ",
    "python functions",
    "",
    " Dictionaries ",
    "DICTIONARIES",
    "  "
]
```

Write a function that:

1. normalizes text
2. removes empty chunks
3. deduplicates chunks
4. preserves first occurrence
5. returns batches of size `batch_size`

Function signature:

```python
def rag_preprocess(chunks: list[str], batch_size: int) -> list[list[str]]:
    pass
```

Expected output:

```python
[
    ["python functions", "dictionaries"]
]
```

when `batch_size=2`.

---

## Strong Solution

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

---

## Explanation

The pipeline is:

```text
raw chunks
↓
normalize
↓
remove empty
↓
deduplicate with set
↓
batch
```

Why normalize before deduplication?

Because these should be considered the same:

```text
" Python   Functions "
"python functions"
"PYTHON FUNCTIONS"
```

Why use a set?

Because set membership is O(1) average.

Why batch?

Because embedding APIs and model inference usually perform better with batches.

---

## Complexity

Let:

```text
n = number of chunks
k = average chunk length
```

Normalization costs O(k) per chunk.

Total:

```text
Time:  O(n * k)
Space: O(n * k)
```

---

## Production Follow-Up

### Interviewer

What would you improve for production?

---

## Strong Answer

For production, I would consider:

* structured logging
* metrics per stage
* max batch size configuration
* input validation
* error handling
* streaming for large datasets
* cache for repeated chunks
* stable chunk IDs
* tenant/user scope
* retries for embedding calls
* storing results in a vector database
* monitoring duplicate rate
* tracing slow stages

---

# Scoring Rubric

Use this rubric to evaluate yourself.

---

## Python Fundamentals

| Score | Meaning                                        |
| ----- | ---------------------------------------------- |
| 1     | Cannot explain references or mutability        |
| 2     | Basic syntax only                              |
| 3     | Understands mutability and data structures     |
| 4     | Explains complexity and tradeoffs              |
| 5     | Connects Python behavior to production systems |

---

## Coding

| Score | Meaning                                    |
| ----- | ------------------------------------------ |
| 1     | Code does not run                          |
| 2     | Works only for sample input                |
| 3     | Handles common cases                       |
| 4     | Handles edge cases and explains complexity |
| 5     | Clean, production-aware, testable solution |

---

## Communication

| Score | Meaning                       |
| ----- | ----------------------------- |
| 1     | Silent or unclear             |
| 2     | Explains only after prompting |
| 3     | Explains basic idea           |
| 4     | Explains tradeoffs clearly    |
| 5     | Thinks like a senior engineer |

---

## AI Engineering Awareness

| Score | Meaning                                                  |
| ----- | -------------------------------------------------------- |
| 1     | No AI connection                                         |
| 2     | Mentions AI generally                                    |
| 3     | Connects to embeddings/RAG basics                        |
| 4     | Explains batching, caching, deduplication                |
| 5     | Discusses production reliability, observability, scaling |

---

# Common Weak Answers

Avoid answers like:

```text
Because Python works like that.
```

Better:

```text
Because assignment copies the reference, not the underlying object.
```

---

Avoid:

```text
Use a dictionary because it is faster.
```

Better:

```text
Use a dictionary because key lookup is O(1) average, which improves the brute-force O(n²) solution to O(n) time at the cost of O(n) extra space.
```

---

Avoid:

```text
The RAG answer is wrong because the LLM hallucinated.
```

Better:

```text
I would first inspect retrieval. If the correct context was not retrieved, the issue may be chunking, embeddings, filters, or ranking. If the right context was retrieved but the answer is still wrong, then I would inspect prompt design and generation settings.
```

---

# Final Self-Assessment

After completing this mock interview, answer these questions:

* [ ] Could I explain variables vs objects clearly?
* [ ] Could I debug mutable default arguments?
* [ ] Could I solve Two Sum without help?
* [ ] Could I explain O(n) vs O(n²)?
* [ ] Could I optimize list membership using a set?
* [ ] Could I write clean functions?
* [ ] Could I debug missing dictionary keys?
* [ ] Could I explain batching in AI systems?
* [ ] Could I build a mini RAG preprocessing pipeline?
* [ ] Could I explain tradeoffs like a senior engineer?

---

# Final Advice

In real interviews, do not rush into code.

Use this structure:

```text
Clarify
↓
Propose brute force
↓
Analyze
↓
Optimize
↓
Code
↓
Test
↓
Explain tradeoffs
```

Strong senior candidates are not only fast coders.

They are clear thinkers.

They explain why their solution is correct, efficient, and maintainable.
