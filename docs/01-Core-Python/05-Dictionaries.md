# Python Dictionaries

> Hash maps, key-value storage, lookup tables, frequency counters, grouping, caching, JSON-like payloads, and production AI engineering patterns.

---

# Why Dictionaries Matter

Dictionaries are one of the most important data structures in Python.

They appear everywhere:

* API payloads
* JSON data
* FastAPI request and response bodies
* Configuration objects
* Caches
* Database result mappings
* Frequency counters
* Grouping problems
* Feature maps
* Model metadata
* RAG document metadata
* Vector search result payloads

In interviews, dictionaries are used to test whether you understand:

* Hash maps
* Fast lookups
* Frequency counting
* Grouping
* Caching
* Time complexity
* Data modeling
* Tradeoffs between list, set, tuple, and dict

A strong Python engineer must be comfortable with dictionaries.

---

# Learning Objectives

By the end of this chapter, you should be able to:

* Explain what a dictionary is
* Explain how dictionaries provide fast key lookup
* Explain keys, values, and hashability
* Use dictionary methods correctly
* Use dictionaries for frequency counting
* Use dictionaries for grouping
* Use dictionaries as lookup tables
* Use dictionaries for caching
* Compare dict vs list vs set vs tuple
* Solve common interview problems using dictionaries
* Use dictionaries safely in production Python and AI systems

---

# What Is A Dictionary?

A dictionary is a collection of key-value pairs.

Example:

```python
user = {
    "name": "Abubaker",
    "role": "Software Engineer",
    "country": "UAE"
}
```

Access by key:

```python
print(user["name"])
```

Output:

```python
Abubaker
```

A dictionary maps:

```text
key -> value
```

Example:

```text
"name"    -> "Abubaker"
"role"    -> "Software Engineer"
"country" -> "UAE"
```

---

# Dictionary Properties

A dictionary is:

* Mutable
* Key-value based
* Fast for lookup by key
* Ordered by insertion order in modern Python
* Requires hashable keys
* Allows values of any type

Example:

```python
data = {
    "id": 101,
    "name": "Invoice OCR",
    "active": True,
    "tags": ["ocr", "ai", "document-ai"],
    "metadata": {
        "owner": "backend-team"
    }
}
```

---

# Visual Model

A list uses indexes:

```text
Index:  0       1       2
Value: "Ali"  "Sara"  "Omar"
```

A dictionary uses keys:

```text
Key           Value
"name"   ->   "Abubaker"
"role"   ->   "Software Engineer"
"city"   ->   "Abu Dhabi"
```

Conceptually:

```text
key ──hash()──► internal location ──► value
```

This is why dictionaries are very fast for lookup.

---

# Creating Dictionaries

## Empty Dictionary

```python
data = {}
```

or:

```python
data = dict()
```

---

## Dictionary With Values

```python
user = {
    "name": "Abubaker",
    "role": "Software Engineer"
}
```

---

## Using dict()

```python
user = dict(
    name="Abubaker",
    role="Software Engineer"
)
```

---

# Accessing Values

```python
user = {
    "name": "Abubaker",
    "role": "Software Engineer"
}

print(user["name"])
```

Output:

```python
Abubaker
```

---

# KeyError

If the key does not exist:

```python
print(user["age"])
```

You get:

```python
KeyError: 'age'
```

This is a common production bug.

---

# Safer Access With get()

```python
age = user.get("age")
```

Output:

```python
None
```

You can provide a default:

```python
age = user.get("age", 0)
```

Output:

```python
0
```

Use `get()` when missing keys are expected.

---

# Adding And Updating Values

```python
user = {}

user["name"] = "Abubaker"
user["role"] = "Software Engineer"
```

Update existing value:

```python
user["role"] = "Senior Software Engineer"
```

---

# Deleting Values

```python
del user["role"]
```

or:

```python
role = user.pop("role")
```

Difference:

```python
del
```

removes the key.

```python
pop()
```

removes and returns the value.

---

# Checking If Key Exists

```python
if "name" in user:
    print("Name exists")
```

This checks keys, not values.

Complexity:

```text
O(1) average
```

---

# Dictionary Methods

## keys()

```python
user.keys()
```

Returns dictionary keys.

---

## values()

```python
user.values()
```

Returns dictionary values.

---

## items()

```python
user.items()
```

Returns key-value pairs.

Example:

```python
for key, value in user.items():
    print(key, value)
```

---

# Iterating Over Dictionaries

By default, iteration goes over keys:

```python
for key in user:
    print(key)
```

Better when you need both key and value:

```python
for key, value in user.items():
    print(key, value)
```

---

# Dictionary Keys

Dictionary keys must be hashable.

Valid keys:

```python
data = {
    "name": "Abubaker",
    1: "one",
    (10, 20): "point"
}
```

Invalid key:

```python
data = {
    ["a", "b"]: "invalid"
}
```

Error:

```python
TypeError: unhashable type: 'list'
```

Lists are mutable, so they cannot be dictionary keys.

---

# Dictionary Values

Dictionary values can be anything:

```python
data = {
    "name": "Abubaker",
    "scores": [90, 85, 95],
    "profile": {
        "country": "UAE"
    },
    "active": True
}
```

Values do not need to be hashable.

Only keys need to be hashable.

---

# How Dictionary Lookup Works

Dictionaries use hash tables.

When you do:

```python
user["name"]
```

Python roughly does:

```text
"name"
  │
  ▼
hash("name")
  │
  ▼
internal location
  │
  ▼
value
```

This makes lookup:

```text
O(1) average case
```

This is why dictionaries are so powerful.

---

# Dictionary Complexity Table

| Operation               | Average Complexity |
| ----------------------- | ------------------ |
| Access by key           | O(1)               |
| Insert key              | O(1)               |
| Update key              | O(1)               |
| Delete key              | O(1)               |
| Membership check by key | O(1)               |
| Iterate over all items  | O(n)               |
| Copy dictionary         | O(n)               |

Important:

These are average-case complexities.

Hash collisions can degrade performance, but normal dictionary operations are extremely efficient.

---

# Dictionary vs List

## List Lookup By Search

```python
users = [
    ("u1", "Ali"),
    ("u2", "Sara"),
    ("u3", "Omar")
]

def find_user(users, user_id):
    for uid, name in users:
        if uid == user_id:
            return name
```

Complexity:

```text
O(n)
```

---

## Dictionary Lookup

```python
users = {
    "u1": "Ali",
    "u2": "Sara",
    "u3": "Omar"
}

name = users["u2"]
```

Complexity:

```text
O(1) average
```

For large systems, this difference is huge.

---

# Dictionary vs Set

Use a set when you only care about existence:

```python
active_users = {"u1", "u2", "u3"}

if "u2" in active_users:
    ...
```

Use a dictionary when you need to map a key to a value:

```python
user_roles = {
    "u1": "admin",
    "u2": "viewer"
}
```

---

# Dictionary vs Tuple

Use tuple for fixed grouped values:

```python
point = (10, 20)
```

Use dictionary when fields need names:

```python
point = {
    "x": 10,
    "y": 20
}
```

Dictionaries are more readable when meaning matters.

---

# Common Pattern 1: Frequency Counter

Very common interview pattern.

Problem:

Count how many times each character appears.

```python
def char_frequency(text):
    freq = {}

    for char in text:
        if char not in freq:
            freq[char] = 0

        freq[char] += 1

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

---

# Frequency Counter With get()

Cleaner:

```python
def char_frequency(text):
    freq = {}

    for char in text:
        freq[char] = freq.get(char, 0) + 1

    return freq
```

This pattern is extremely useful.

---

# Frequency Counter With Counter

```python
from collections import Counter

freq = Counter("banana")
```

Output:

```python
Counter({'a': 3, 'n': 2, 'b': 1})
```

Use `Counter` when frequency counting is the main goal.

---

# Common Pattern 2: Grouping

Problem:

Group words by length.

```python
def group_by_length(words):
    groups = {}

    for word in words:
        length = len(word)

        if length not in groups:
            groups[length] = []

        groups[length].append(word)

    return groups
```

Example:

```python
words = ["AI", "RAG", "Python", "ML", "FastAPI"]

print(group_by_length(words))
```

Output:

```python
{
    2: ["AI", "ML"],
    3: ["RAG"],
    6: ["Python"],
    7: ["FastAPI"]
}
```

---

# Grouping With setdefault()

```python
def group_by_length(words):
    groups = {}

    for word in words:
        groups.setdefault(len(word), []).append(word)

    return groups
```

This works, but some engineers prefer explicit code for readability.

---

# Grouping With defaultdict()

```python
from collections import defaultdict

def group_by_length(words):
    groups = defaultdict(list)

    for word in words:
        groups[len(word)].append(word)

    return dict(groups)
```

This is clean and common in interviews.

---

# Common Pattern 3: Lookup Table

Instead of many if statements:

```python
def get_status(code):
    if code == 200:
        return "OK"
    elif code == 404:
        return "Not Found"
    elif code == 500:
        return "Server Error"
```

Use a dictionary:

```python
STATUS_CODES = {
    200: "OK",
    404: "Not Found",
    500: "Server Error"
}

def get_status(code):
    return STATUS_CODES.get(code, "Unknown")
```

This is cleaner and easier to extend.

---

# Common Pattern 4: Caching

Dictionaries are often used for caching.

```python
cache = {}

def expensive_function(x):
    if x in cache:
        return cache[x]

    result = x * x

    cache[x] = result

    return result
```

This avoids repeated computation.

---

# Common Pattern 5: Two Sum

Classic interview problem.

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

Why dictionary?

Because it allows O(1) average lookup.

Complexity:

```text
Time: O(n)
Space: O(n)
```

---

# Common Pattern 6: Group Anagrams

Problem:

Group words that are anagrams.

```python
from collections import defaultdict

def group_anagrams(words):
    groups = defaultdict(list)

    for word in words:
        key = tuple(sorted(word))
        groups[key].append(word)

    return list(groups.values())
```

Example:

```python
words = ["eat", "tea", "tan", "ate", "nat", "bat"]

print(group_anagrams(words))
```

Output:

```python
[
    ["eat", "tea", "ate"],
    ["tan", "nat"],
    ["bat"]
]
```

---

# Common Pattern 7: Inverting A Dictionary

```python
user_to_role = {
    "u1": "admin",
    "u2": "viewer"
}

role_to_users = {}

for user_id, role in user_to_role.items():
    role_to_users.setdefault(role, []).append(user_id)
```

Result:

```python
{
    "admin": ["u1"],
    "viewer": ["u2"]
}
```

---

# Nested Dictionaries

Dictionaries can contain dictionaries.

```python
users = {
    "u1": {
        "name": "Ali",
        "role": "admin"
    },
    "u2": {
        "name": "Sara",
        "role": "viewer"
    }
}
```

Access:

```python
users["u1"]["role"]
```

Output:

```python
admin
```

Nested dictionaries are common in JSON APIs.

---

# Dictionary Comprehensions

Example:

```python
nums = [1, 2, 3]

squares = {
    num: num * num
    for num in nums
}
```

Output:

```python
{
    1: 1,
    2: 4,
    3: 9
}
```

---

# Filtering With Dictionary Comprehension

```python
scores = {
    "Ali": 90,
    "Sara": 75,
    "Omar": 60
}

passed = {
    name: score
    for name, score in scores.items()
    if score >= 70
}
```

Output:

```python
{
    "Ali": 90,
    "Sara": 75
}
```

---

# Merging Dictionaries

## Using update()

```python
a = {"name": "Abubaker"}
b = {"role": "Software Engineer"}

a.update(b)
```

Result:

```python
{
    "name": "Abubaker",
    "role": "Software Engineer"
}
```

---

## Using |

```python
a = {"name": "Abubaker"}
b = {"role": "Software Engineer"}

merged = a | b
```

Result:

```python
{
    "name": "Abubaker",
    "role": "Software Engineer"
}
```

---

# Common Mistake 1: Accessing Missing Keys

Bad:

```python
country = user["country"]
```

If `country` is missing, this raises `KeyError`.

Better:

```python
country = user.get("country", "Unknown")
```

---

# Common Mistake 2: Mutable Default Dictionary

Bad:

```python
def add_metadata(key, value, metadata={}):
    metadata[key] = value
    return metadata
```

Problem:

The same dictionary is reused across calls.

Correct:

```python
def add_metadata(key, value, metadata=None):
    if metadata is None:
        metadata = {}

    metadata[key] = value

    return metadata
```

---

# Common Mistake 3: Modifying Dictionary While Iterating

Bad:

```python
for key in data:
    if data[key] is None:
        del data[key]
```

This can raise an error.

Better:

```python
keys_to_delete = []

for key, value in data.items():
    if value is None:
        keys_to_delete.append(key)

for key in keys_to_delete:
    del data[key]
```

Or use comprehension:

```python
data = {
    key: value
    for key, value in data.items()
    if value is not None
}
```

---

# Common Mistake 4: Using Dict When Order Or Duplicates Matter

Dictionaries map unique keys to values.

If duplicate keys appear, later values overwrite earlier values.

```python
data = {
    "name": "Ali",
    "name": "Sara"
}
```

Result:

```python
{
    "name": "Sara"
}
```

The first value is lost.

---

# Production Engineering Notes

Dictionaries are everywhere in backend systems.

Examples:

## FastAPI Response

```python
return {
    "status": "success",
    "data": result
}
```

## Configuration

```python
config = {
    "model": "gpt-4",
    "temperature": 0.2,
    "timeout": 30
}
```

## Cache

```python
user_cache = {
    "user_123": user_profile
}
```

## Routing Table

```python
handlers = {
    "invoice.created": handle_invoice_created,
    "payment.completed": handle_payment_completed
}
```

---

# AI Engineering Examples

## Example 1: Document Metadata

```python
document = {
    "id": "doc_123",
    "text": "Python dictionaries are hash maps.",
    "metadata": {
        "source": "python-handbook",
        "topic": "core-python",
        "created_at": "2026-06-02"
    }
}
```

This pattern appears in vector databases and RAG pipelines.

---

## Example 2: Embedding Cache

```python
embedding_cache = {}

def get_embedding(text):
    if text in embedding_cache:
        return embedding_cache[text]

    embedding = embed(text)

    embedding_cache[text] = embedding

    return embedding
```

This saves repeated model calls.

In production, use:

* Redis
* Disk cache
* Vector database
* TTL cache
* LRU cache

instead of an unbounded in-memory dictionary.

---

## Example 3: Model Registry

```python
models = {
    "embedding-small": small_embedding_model,
    "embedding-large": large_embedding_model,
    "reranker": reranker_model
}

model = models["embedding-small"]
```

This avoids long conditional chains.

---

## Example 4: RAG Retrieval Result

```python
result = {
    "document_id": "doc_123",
    "score": 0.91,
    "text": "Python dictionaries are useful.",
    "metadata": {
        "source": "core-python",
        "page": 5
    }
}
```

Dictionaries are often the natural format for search results.

---

# Senior Deep Dive: Dictionaries And Memory

Dictionaries are fast because they use extra memory.

A dictionary is usually less memory-efficient than a compact list.

Tradeoff:

```text
More memory
↓
Faster lookup
```

This is usually worth it when lookup performance matters.

But for millions of records, memory usage becomes important.

Possible alternatives:

* Database
* Redis
* SQLite
* Parquet
* NumPy arrays
* Pandas DataFrames
* Specialized indexes

---

# Senior Deep Dive: Unbounded Cache Problem

This is a common production issue.

Bad:

```python
cache = {}

def get_result(query):
    if query not in cache:
        cache[query] = expensive_call(query)

    return cache[query]
```

Problem:

The cache grows forever.

In a long-running service, this can cause:

* Memory growth
* OOM crashes
* Slow garbage collection
* Poor container stability

Better:

```python
from functools import lru_cache

@lru_cache(maxsize=1000)
def get_result(query):
    return expensive_call(query)
```

Or use a real cache:

* Redis
* Memcached
* TTL cache
* LRU cache

---

# Senior Deep Dive: Key Design

Choosing dictionary keys is a design decision.

Bad key:

```python
cache[text] = embedding
```

Problem:

Text may be huge.

Better key:

```python
import hashlib

key = hashlib.sha256(text.encode()).hexdigest()

cache[key] = embedding
```

Even better:

```python
key = (model_name, text_hash)
```

This avoids mixing embeddings from different models.

---

# Interview Questions And Answers

## Q1. What is a dictionary?

A dictionary is a mutable key-value data structure.

It maps keys to values.

Example:

```python
user = {
    "name": "Abubaker",
    "role": "Software Engineer"
}
```

---

## Q2. Why are dictionaries fast?

Dictionaries use hash tables.

Python computes the hash of a key and uses it to locate the value efficiently.

Average lookup is O(1).

---

## Q3. What types can be dictionary keys?

Keys must be hashable.

Common valid keys:

* strings
* integers
* floats
* booleans
* tuples containing only hashable values

Invalid keys:

* lists
* dictionaries
* sets

---

## Q4. Difference between `dict["key"]` and `dict.get("key")`?

```python
dict["key"]
```

raises `KeyError` if missing.

```python
dict.get("key")
```

returns `None` or a default value if missing.

---

## Q5. How do you count frequencies?

```python
freq = {}

for item in items:
    freq[item] = freq.get(item, 0) + 1
```

Or:

```python
from collections import Counter

freq = Counter(items)
```

---

## Q6. How do you group values?

```python
from collections import defaultdict

groups = defaultdict(list)

for item in items:
    groups[key(item)].append(item)
```

---

## Q7. What is `setdefault()`?

`setdefault()` returns the value for a key if it exists.

If the key does not exist, it inserts the key with a default value.

Example:

```python
groups.setdefault("python", []).append("dict")
```

---

## Q8. What is defaultdict?

`defaultdict` automatically creates a default value for missing keys.

Example:

```python
from collections import defaultdict

groups = defaultdict(list)

groups["a"].append(1)
```

No manual initialization needed.

---

## Q9. Can dictionaries preserve order?

Modern Python dictionaries preserve insertion order.

However, dictionaries should primarily be used for mapping.

If order-specific behavior is central to the design, make that design clear.

---

## Q10. When should you avoid dictionaries?

Avoid dictionaries when:

* You need duplicate keys
* You need compact numeric computation
* You need strict schema validation
* You need object behavior
* You need persistent storage

Consider:

* list
* tuple
* dataclass
* Pydantic model
* database
* NumPy
* Pandas

---

# Senior-Level Questions And Answers

## Senior Q1. Explain how a dictionary works internally.

A dictionary uses a hash table.

When a key is inserted, Python computes its hash and uses that to determine where the key-value pair should be stored.

When looking up a key, Python computes the hash again and checks the expected location.

This gives O(1) average lookup.

---

## Senior Q2. What is a hash collision?

A hash collision happens when two different keys map to the same internal location.

Python handles collisions internally.

Average dictionary performance remains O(1), but worst-case behavior can degrade.

---

## Senior Q3. Why are mutable objects not valid dictionary keys?

Mutable objects can change after insertion.

If a key's hash changed, the dictionary would no longer know where to find it.

That is why lists, dictionaries, and sets are unhashable.

---

## Senior Q4. How would you design a cache key for embeddings?

A strong key should include:

* model name
* normalized text hash
* embedding version
* tenant/user scope if needed

Example:

```python
key = (
    model_name,
    embedding_version,
    hashlib.sha256(text.encode()).hexdigest()
)
```

This prevents returning embeddings from the wrong model or version.

---

## Senior Q5. Why might a dictionary cause memory problems?

Dictionaries use extra memory for hash table performance.

An unbounded dictionary cache can grow forever in a long-running service.

This can lead to memory leaks, OOM crashes, and unstable containers.

---

## Senior Q6. How would you remove keys with None values safely?

Use dictionary comprehension:

```python
cleaned = {
    key: value
    for key, value in data.items()
    if value is not None
}
```

Do not delete keys while iterating directly over the dictionary.

---

## Senior Q7. Dict vs Pydantic model in FastAPI?

Use dictionary for lightweight dynamic data.

Use Pydantic when you need:

* validation
* schema
* serialization
* documentation
* clear API contracts

For production APIs, Pydantic models are usually better than raw dictionaries.

---

# Exercises

## Exercise 1 — Create User Dictionary

Create a dictionary representing a user with:

```text
name
role
country
is_active
```

---

## Exercise 2 — Safe Access

Given:

```python
user = {"name": "Abubaker"}
```

Safely get `country` with default `"Unknown"`.

---

## Exercise 3 — Character Frequency

Implement:

```python
def char_frequency(text):
    pass
```

---

## Exercise 4 — Word Frequency

Implement:

```python
def word_frequency(sentence):
    pass
```

---

## Exercise 5 — First Non-Repeating Character

Input:

```python
"swiss"
```

Output:

```python
"w"
```

---

## Exercise 6 — Group Words By Length

Input:

```python
["AI", "ML", "Python", "RAG"]
```

Output:

```python
{
    2: ["AI", "ML"],
    3: ["RAG"],
    6: ["Python"]
}
```

---

## Exercise 7 — Two Sum

Implement:

```python
def two_sum(nums, target):
    pass
```

---

## Exercise 8 — Merge Two Dictionaries

Merge:

```python
a = {"name": "Abubaker"}
b = {"role": "Software Engineer"}
```

---

## Exercise 9 — Invert Dictionary

Input:

```python
{"u1": "admin", "u2": "viewer"}
```

Output:

```python
{"admin": ["u1"], "viewer": ["u2"]}
```

---

## Exercise 10 — Remove None Values

Input:

```python
{
    "name": "Abubaker",
    "age": None,
    "country": "UAE"
}
```

Output:

```python
{
    "name": "Abubaker",
    "country": "UAE"
}
```

---

## Exercise 11 — Group Anagrams

Input:

```python
["eat", "tea", "tan", "ate", "nat", "bat"]
```

Output:

```python
[
    ["eat", "tea", "ate"],
    ["tan", "nat"],
    ["bat"]
]
```

---

## Exercise 12 — Simple Cache

Implement a simple dictionary-based cache for an expensive function.

---

## Exercise 13 — Count Document Topics

Given a list of document dictionaries:

```python
docs = [
    {"id": "d1", "topic": "python"},
    {"id": "d2", "topic": "ai"},
    {"id": "d3", "topic": "python"}
]
```

Return:

```python
{
    "python": 2,
    "ai": 1
}
```

---

## Exercise 14 — Build ID Lookup

Given users:

```python
users = [
    {"id": "u1", "name": "Ali"},
    {"id": "u2", "name": "Sara"}
]
```

Return:

```python
{
    "u1": {"id": "u1", "name": "Ali"},
    "u2": {"id": "u2", "name": "Sara"}
}
```

---

## Exercise 15 — Embedding Cache Key

Create a function that builds a safe embedding cache key using:

```text
model_name
embedding_version
text
```

---

# Solutions

## Solution 1

```python
user = {
    "name": "Abubaker",
    "role": "Software Engineer",
    "country": "UAE",
    "is_active": True
}
```

---

## Solution 2

```python
country = user.get("country", "Unknown")
```

---

## Solution 3

```python
def char_frequency(text):
    freq = {}

    for char in text:
        freq[char] = freq.get(char, 0) + 1

    return freq
```

---

## Solution 4

```python
def word_frequency(sentence):
    freq = {}

    for word in sentence.lower().split():
        freq[word] = freq.get(word, 0) + 1

    return freq
```

---

## Solution 5

```python
def first_non_repeating_char(text):
    freq = {}

    for char in text:
        freq[char] = freq.get(char, 0) + 1

    for char in text:
        if freq[char] == 1:
            return char

    return None
```

---

## Solution 6

```python
def group_words_by_length(words):
    groups = {}

    for word in words:
        length = len(word)

        if length not in groups:
            groups[length] = []

        groups[length].append(word)

    return groups
```

With defaultdict:

```python
from collections import defaultdict

def group_words_by_length(words):
    groups = defaultdict(list)

    for word in words:
        groups[len(word)].append(word)

    return dict(groups)
```

---

## Solution 7

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

## Solution 8

```python
a = {"name": "Abubaker"}
b = {"role": "Software Engineer"}

merged = a | b
```

Alternative:

```python
merged = {**a, **b}
```

---

## Solution 9

```python
def invert_roles(user_to_role):
    role_to_users = {}

    for user_id, role in user_to_role.items():
        if role not in role_to_users:
            role_to_users[role] = []

        role_to_users[role].append(user_id)

    return role_to_users
```

---

## Solution 10

```python
def remove_none_values(data):
    return {
        key: value
        for key, value in data.items()
        if value is not None
    }
```

---

## Solution 11

```python
from collections import defaultdict

def group_anagrams(words):
    groups = defaultdict(list)

    for word in words:
        key = tuple(sorted(word))
        groups[key].append(word)

    return list(groups.values())
```

---

## Solution 12

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

## Solution 13

```python
def count_topics(docs):
    counts = {}

    for doc in docs:
        topic = doc["topic"]
        counts[topic] = counts.get(topic, 0) + 1

    return counts
```

---

## Solution 14

```python
def build_user_lookup(users):
    return {
        user["id"]: user
        for user in users
    }
```

---

## Solution 15

```python
import hashlib

def build_embedding_cache_key(model_name, embedding_version, text):
    normalized_text = text.strip().lower()
    text_hash = hashlib.sha256(
        normalized_text.encode("utf-8")
    ).hexdigest()

    return (
        model_name,
        embedding_version,
        text_hash
    )
```

---

# Mock Interview

## Interviewer

What is a dictionary?

## Strong Candidate Answer

A dictionary is a mutable key-value data structure. It maps hashable keys to values and provides O(1) average lookup by key.

---

## Interviewer

Why are dictionaries fast?

## Strong Candidate Answer

They use hash tables. Python computes the hash of a key and uses it to locate the value efficiently, so lookup is O(1) on average.

---

## Interviewer

Can a list be a dictionary key?

## Strong Candidate Answer

No. Lists are mutable and unhashable. Dictionary keys must be hashable so their location in the hash table remains stable.

---

## Interviewer

How would you count word frequency?

## Strong Candidate Answer

I would use a dictionary and increment each word count:

```python
freq[word] = freq.get(word, 0) + 1
```

Or use `collections.Counter`.

---

## Interviewer

How would you group documents by topic?

## Strong Candidate Answer

I would use a dictionary where the topic is the key and the value is a list of documents.

```python
groups.setdefault(topic, []).append(doc)
```

For cleaner code, I might use `defaultdict(list)`.

---

## Interviewer

How would dictionaries appear in a RAG system?

## Strong Candidate Answer

Dictionaries appear in metadata, retrieval results, caches, model registries, configuration, and API responses. For example, each retrieved chunk might be represented as a dictionary containing document ID, text, score, and metadata.

---

## Interviewer

What is a common production risk with dictionary caches?

## Strong Candidate Answer

An unbounded dictionary cache can grow forever in a long-running service and cause high memory usage or OOM crashes. I would use LRU cache, TTL cache, Redis, or another controlled caching system.

---

# Revision Sheet

## Dictionary Basics

```python
user = {
    "name": "Abubaker",
    "role": "Software Engineer"
}
```

---

## Access

```python
user["name"]
```

May raise `KeyError`.

Safer:

```python
user.get("name")
```

---

## Add / Update

```python
user["country"] = "UAE"
```

---

## Delete

```python
del user["country"]
```

or:

```python
user.pop("country")
```

---

## Iterate

```python
for key, value in user.items():
    print(key, value)
```

---

## Frequency Count

```python
freq[item] = freq.get(item, 0) + 1
```

---

## Grouping

```python
groups.setdefault(key, []).append(value)
```

or:

```python
from collections import defaultdict

groups = defaultdict(list)
```

---

## Merge

```python
merged = a | b
```

---

## Dictionary Comprehension

```python
squares = {
    x: x * x
    for x in nums
}
```

---

# Cheat Sheet

| Task       | Code                                 |
| ---------- | ------------------------------------ |
| Empty dict | `{}`                                 |
| Get value  | `d[key]`                             |
| Safe get   | `d.get(key, default)`                |
| Add/update | `d[key] = value`                     |
| Delete     | `del d[key]`                         |
| Pop        | `d.pop(key)`                         |
| Keys       | `d.keys()`                           |
| Values     | `d.values()`                         |
| Items      | `d.items()`                          |
| Key exists | `key in d`                           |
| Merge      | `a \| b`                             |
| Count      | `freq[x] = freq.get(x, 0) + 1`       |
| Group      | `groups.setdefault(k, []).append(v)` |

---

# Final Key Takeaways

Dictionaries are one of the most important Python data structures.

Use dictionaries when you need:

* Fast lookup
* Key-value mapping
* Frequency counting
* Grouping
* Caching
* JSON-like data
* Metadata storage
* Lookup tables

Avoid dictionaries when you need:

* Duplicate keys
* Ordered numeric arrays
* Strict schemas without validation
* Persistent storage
* Memory-efficient numeric computation

For interviews, remember:

```text
Dictionary lookup: O(1) average
List search: O(n)
```

Many interview problems become simple and efficient once you recognize that a dictionary is the right tool.
