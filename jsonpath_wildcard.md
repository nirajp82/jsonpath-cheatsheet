### 🌟 JSONPath Wildcard `*`
The `*` wildcard in JSONPath is used to match all elements or properties at a specific level, much like a glob in shell scripting. It’s a powerful tool for querying unknown or dynamic data structures.

#### 🧾 Usage Examples

1. **📋 Match all items in an array**
```jsonpath
$.users[*]
```
**Goal**: Return every user object in the `users` array.

**Result**:
```json
[
  { "id": 1, "name": "Alice", ... },
  { "id": 2, "name": "Bob", ... },
  { "id": 3, "name": "Charlie", ... }
]
```

2. **🔑 Match all properties of an object**
```jsonpath
$.users[0].*
```
**Goal**: Return all values of the first user’s properties.

**Result**:
```json
[1, "Alice", "alice@example.com", ["admin", "editor"], [...orders...]]
```

3. **📦 Match all nested orders across users**
```jsonpath
$..orders[*]
```
**Goal**: Return every order object from all users.

**Result**:
```json
[
  { "id": 1001, "amount": 250, "status": "shipped" },
  { "id": 1002, "amount": 120, "status": "processing" },
  { "id": 1003, "amount": 300, "status": "shipped" }
]
```

4. **📧 Get all user emails**
```jsonpath
$.users[*].email
```
**Goal**: Extract all email values from the `users` array.

**Result**:
```json
["alice@example.com", "bob@example.com", "charlie@example.com"]
```

---

### 📘 Summary of Wildcard `*` Usage

| Expression            | Description                                             |
|-----------------------|---------------------------------------------------------|
| `$.*`                 | All properties of the root object                      |
| `$..*`                | All properties at all levels recursively               |
| `$.users[*]`          | All user objects                                        |
| `$.users[*].name`     | All user names                                          |
| `$.users[0].*`        | All values of the first user's properties              |
| `$..orders[*].amount` | All order amounts regardless of user                   |
