## 🌟 JSONPath Wildcard `*`

The `*` wildcard in JSONPath is used to match all elements or properties at a specific level—similar to a glob in shell scripting. It’s a powerful tool for querying unknown, dynamic, or multi-key JSON structures.

---

### 📘 Sample JSON Data (User-Focused)

This is a **real-world e-commerce user data** JSON:

```json
{
  "users": [
    {
      "id": 1,
      "name": "Alice",
      "email": "alice@example.com",
      "roles": ["admin", "editor"],
      "orders": [
        { "id": 1001, "amount": 250, "status": "shipped" },
        { "id": 1002, "amount": 120, "status": "processing" }
      ]
    },
    {
      "id": 2,
      "name": "Bob",
      "email": "bob@example.com",
      "roles": ["viewer"],
      "orders": [
        { "id": 1003, "amount": 300, "status": "shipped" }
      ]
    },
    {
      "id": 3,
      "name": "Charlie",
      "email": "charlie@example.com",
      "roles": [],
      "orders": []
    }
  ]
}
```

---

### 🧾 Usage Examples

#### 1. **📋 Match all items in an array**
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

---

#### 2. **🔑 Match all properties of an object**
```jsonpath
$.users[0].*
```
**Goal**: Return all values of the first user’s properties.

**Result**:
```json
[1, "Alice", "alice@example.com", ["admin", "editor"], [...orders...]]
```

---

#### 3. **📦 Match all nested orders across users**
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

---

#### 4. **📧 Get all user emails**
```jsonpath
$.users[*].email
```
**Goal**: Extract all email values from the `users` array.

**Result**:
```json
["alice@example.com", "bob@example.com", "charlie@example.com"]
```

---

### 🚌 Scenario: Extracting Nested Values from Multiple Objects

📘 **Sample JSON (`q2.json`)**:
```json
{
    "car": {
        "color": "blue",
        "price": "$20,000"
    },
    "bus": {
        "color": "white",
        "price": "$120,000"
    }
}
```

#### 🎯 Goal: Get the `color` property of all top-level objects

```jsonpath
$.*.color
```

✅ **Result**:
```json
["blue", "white"]
```

🧠 **Explanation**:
- `$.*` selects all top-level properties (`car`, `bus`).
- `.color` accesses the `color` property within each of those objects.

---

### 🚗🚌 Scenario: Accessing Nested Properties Under a Named Parent

📘 **Sample JSON (`q3.json`)**:
```json
{
    "vehicles": {
        "car": {
            "color": "blue",
            "price": "$20,000"
        },
        "bus": {
            "color": "white",
            "price": "$120,000"
        }
    }
}
```

#### 🎯 Goal: Get the `price` of all vehicle types (car, bus)

```jsonpath
$.vehicles.*.price
```

✅ **Result**:
```json
["$20,000", "$120,000"]
```

🧠 **Explanation**:
- `$.vehicles` targets the main container.
- `*` selects all child objects (`car`, `bus`).
- `.price` extracts the `price` field from each.

---

### 🧠 Summary of Wildcard `*` Usage

| Expression              | Description                                                |
|-------------------------|------------------------------------------------------------|
| `$.*`                   | All properties of the root object                          |
| `$..*`                  | All properties at all levels recursively                   |
| `$.users[*]`            | All user objects in an array                               |
| `$.users[*].name`       | All user names                                             |
| `$.users[0].*`          | All values of the first user's properties                  |
| `$..orders[*].amount`   | All order amounts regardless of which user made the order  |
| `$.*.color`             | All `color` properties from top-level objects (q2 example) |
| `$.vehicles.*.price`    | All `price` values inside nested vehicle objects (q3)      |
