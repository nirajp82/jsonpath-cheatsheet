## 🔰 What is JSONPath?

**JSONPath** is a query language for JSON, similar to XPath for XML. It lets you extract and filter specific parts of a JSON document using path expressions.

---
## Complete JSONPath Cheat Sheet

### 📘 Sample JSON Data

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

### 🔎 JSONPath Queries with Titles, Goals & Results

---

### 1. 🔍 **Return the entire JSON document**

**Query:**
```jsonpath
$
```

**Goal:** Select the full root JSON object.

**Result:**  
```json
{ "users": [...] }
```

---

### 2. 👤 **Get all user names**

**Query:**
```jsonpath
$.users[*].name
```

**Goal:** Fetch the `name` field of every user in the array.

**Result:**  
```json
["Alice", "Bob", "Charlie"]
```

---

### 3. 🧍 **Get the first user object**

**Query:**
```jsonpath
$.users[0]
```

**Goal:** Return the first user in the `users` array.

**Result:**  
```json
{
  "id": 1,
  "name": "Alice",
  ...
}
```

---

### 4. 🔚 **Get the last user's name using slicing**

**Query:**
```jsonpath
$.users[-1:].name
```

**Goal:** Return the name of the last user.

**Result:**  
```json
["Charlie"]
```

---

### 5. ❌ **Find users with no roles assigned**

**Query:**
```jsonpath
$.users[?(@.roles.length == 0)].name
```

**Goal:** Return names of users where the `roles` array is empty.

**Result:**  
```json
["Charlie"]
```

---

### 6. 💸 **Find all orders greater than $200**

**Query:**
```jsonpath
$..orders[?(@.amount > 200)].id
```

**Goal:** Get IDs of orders where `amount > 200`.

**Result:**  
```json
[1001, 1003]
```

---

### 7. 📦 **Find all shipped orders**

**Query:**
```jsonpath
$..orders[?(@.status == 'shipped')]
```

**Goal:** Return order objects where `status` is `"shipped"`.

**Result:**  
```json
[ 
  { "id": 1001, "amount": 250, "status": "shipped" },
  { "id": 1003, "amount": 300, "status": "shipped" }
]
```

---

### 8. 💰 **Get all order amounts**

**Query:**
```jsonpath
$..orders[*].amount
```

**Goal:** List the `amount` of every order from all users.

**Result:**  
```json
[250, 120, 300]
```

---

### 9. 🥇 **Get the first order from each user**

**Query:**
```jsonpath
$..orders[0]
```

**Goal:** Return the first order from each user's `orders` array (if any).

**Result:**  
```json
[ 
  { "id": 1001, "amount": 250, "status": "shipped" },
  { "id": 1003, "amount": 300, "status": "shipped" }
]
```

---

### 10. 📧 **Find all objects with an email field**

**Query:**
```jsonpath
$..[?(@.email)]
```

**Goal:** Filter and return only objects that contain an `email` key.

**Result:**  
```json
[ 
  { "id": 1, "name": "Alice", "email": "alice@example.com", ... },
  { "id": 2, "name": "Bob", "email": "bob@example.com", ... },
  { "id": 3, "name": "Charlie", "email": "charlie@example.com", ... }
]
```

---

### 11. 👑 **Find users with the 'admin' role**

**Query:**
```jsonpath
$.users[?(@.roles.indexOf('admin') != -1)].name
```

**Goal:** Return names of users who have the `admin` role in their roles array.

**Result:**  
```json
["Alice"]
```

---

### 12. 🧪 **Regex match: Get orders with "ship" in status**

**Query:**
```jsonpath
$..orders[?(@.status =~ /ship/i)]
```

**Goal:** Use regex to find all orders whose `status` contains `"ship"` (case-insensitive).

**Result:**  
```json
[ 
  { "id": 1001, "amount": 250, "status": "shipped" },
  { "id": 1003, "amount": 300, "status": "shipped" }
]
```

> ⚠️ Note: Regex filters like `=~` are supported in `jsonpath-plus`, `Jayway`, but not all engines.

---

### 13. 🔄 **Find users with both `shipped` and `processing` orders**

**Query:**
```jsonpath
$.users[?(
  @.orders[?(@.status == 'shipped')] &&
  @.orders[?(@.status == 'processing')]
)]
```

**Goal:**  
Return **user objects** where the user has **both `shipped` and `processing` orders**.

**Result:**  
```json
[ 
  {
    "id": 1,
    "name": "Alice",
    "email": "alice@example.com",
    "roles": ["admin", "editor"],
    "orders": [
      { "id": 1001, "amount": 250, "status": "shipped" },
      { "id": 1002, "amount": 120, "status": "processing" }
    ]
  }
]
```

---

### 14. 📦 **Fetch only the order details where the order is `shipped` or `processing`**

**Query:**
```jsonpath
$..orders[?(@.status == 'shipped' || @.status == 'processing')]
```

**Goal:**  
Return only the **order objects** where the `status` is either `shipped` or `processing`.

**Result:**  
```json
[ 
  { "id": 1001, "amount": 250, "status": "shipped" },
  { "id": 1002, "amount": 120, "status": "processing" },
  { "id": 1003, "amount": 300, "status": "shipped" }
]
```
### 15. 📦 **Fetch only the order details where the order is `shipped` or `processing`**

**Query:**
```jsonpath
$..orders[?(@.status == 'shipped' || @.status == 'processing')]
```

**Goal:**  
Return only the **order objects** that have a `status` of either `shipped` or `processing`.

---

**Explanation:**
- The `||` (OR) operator is used to ensure that the `status` of the order matches either `shipped` or `processing`.
- This query filters out all orders that do not meet either of these two conditions.

---

**Result:**  
```json
[
  { "id": 1001, "amount": 250, "status": "shipped" },
  { "id": 1002, "amount": 120, "status": "processing" },
  { "id": 1003, "amount": 300, "status": "shipped" }
]
```

**Explanation:**
- The query returns all orders with a status of `shipped` or `processing`, **regardless of which user made them**.
- All three orders are returned, as their statuses are either `shipped` or `processing`.

---

## 🧠 JSONPath Operator Reference

| Syntax                   | Description                                 |
|--------------------------|---------------------------------------------|
| `$`                      | Root of the document                        |
| `.`                      | Child element                               |
| `..`                     | Recursive search                            |
| `[*]`                    | All elements                                |
| `[n]`                    | Array index                                 |
| `[-1:]`                  | Slice (e.g., last item)                     |
| `[?(@.key)]`             | Filter: has property                        |
| `[?(@.key == value)]`    | Filter: equals                              |
| `[?(@.array.length == 0)]` | Filter: empty array                     |
| `[?(@.status =~ /.../)]` | Regex filter                                |
| `@`                      | Refers to current node in filter            |

---

For **.NET** developers working with **JSONPath**, there are a couple of tools you can use:

### 1. **JsonPath.Net**
   - **Description**: A .NET library for querying JSON data using JSONPath expressions.
   - **GitHub Repository**: [JsonPath.Net GitHub](https://github.com/dajuric/JsonPath.Net)
   - **NuGet Package**: [JsonPath.Net NuGet](https://www.nuget.org/packages/JsonPath)

   **Usage Example** (C#):
   ```csharp
   using JsonPath;

   var json = @"{
       'users': [
           { 'name': 'Alice', 'age': 30 },
           { 'name': 'Bob', 'age': 25 }
       ]
   }";

   var path = "$.users[?(@.age > 26)]";
   var result = JsonPath.Parse(json).SelectTokens(path);
   ```

### 2. **Newtonsoft.Json (Json.NET) with JSONPath support**
   - **Description**: While `Newtonsoft.Json` is primarily known for JSON parsing and manipulation in .NET, it also supports **JSONPath** queries using the `SelectToken()` method.
   - **NuGet Package**: [Newtonsoft.Json NuGet](https://www.nuget.org/packages/Newtonsoft.Json)

   **Usage Example** (C#):
   ```csharp
   using Newtonsoft.Json.Linq;

   var json = @"{
       'users': [
           { 'name': 'Alice', 'age': 30 },
           { 'name': 'Bob', 'age': 25 }
       ]
   }";

   var jObject = JObject.Parse(json);
   var result = jObject.SelectTokens("$.users[?(@.age > 26)]");

   foreach (var item in result)
   {
       Console.WriteLine(item);
   }
   ```

   **Note**: `Newtonsoft.Json` is a very popular library for working with JSON in .NET, and its built-in support for **JSONPath** makes it easy to perform queries on JSON data.

### 3. **JPath**
   - **Description**: JPath is another JSONPath library designed specifically for **.NET**.
   - **GitHub Repository**: [JPath GitHub](https://github.com/mganss/JPath)
   - **NuGet Package**: [JPath NuGet](https://www.nuget.org/packages/JPath)

   **Usage Example**:
   ```csharp
   using JPath;
   using Newtonsoft.Json.Linq;

   var json = @"{
       'users': [
           { 'name': 'Alice', 'age': 30 },
           { 'name': 'Bob', 'age': 25 }
       ]
   }";

   var jObject = JObject.Parse(json);
   var result = jObject.SelectTokens("$.users[?(@.age > 26)]");

   foreach (var item in result)
   {
       Console.WriteLine(item);
   }
   ```

---

### Which Tool to Choose?

- **For most .NET developers**, the **Newtonsoft.Json** library is the go-to choice due to its extensive functionality, wide usage, and built-in JSONPath support through the `SelectTokens()` method. It's a versatile and robust library for working with JSON in .NET.
  
- If you prefer a dedicated **JSONPath** implementation with an API tailored for querying JSON, you can consider **JsonPath.Net** or **JPath**.

## 🛠 Other tools to Try JSONPath

- [https://jsonpath.com](https://jsonpath.com)
- [https://jsonformatter.org/jsonpath](https://jsonformatter.org/jsonpath)
- NPM: [`jsonpath-plus`](https://www.npmjs.com/package/jsonpath-plus)
- Python: `jsonpath-ng`
- Java: Jayway JSONPath
