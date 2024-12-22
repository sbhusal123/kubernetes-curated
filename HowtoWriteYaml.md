# How to Write A Yaml File

## 1. Key-Value Pairs:

Key-value pairs are used to define properties of Kubernetes resources.

```yaml
key: value
```

Equivalent JSON would be.

```json
{
  "key": "value"
}

```

## 2. Nested Structures

Nested structures can be represented using indentation in YAML

```yml
parent:
  child: value
  foo: bar
```

Equivalent JSON:

```json
{
  "parent": {
    "child": "value",
    "foo": "bar"
  }
}
```

## 3. List

List item represented by: ``-``

```yaml
items:
  - item1
  - item2
  - item3
```

Equivalent JSON.

```json
{
  "items": ["item1", "item2", "item3"]
}
```

## 4. Multiple Key Value Pairs:

Notice the indentation under the key.

```yml
user:
  name: John
  age: 30
  location: New York
```

Equivalent JSON would be:

```json
{
  "user": {
    "name": "John",
    "age": 30,
    "location": "New York"
  }
}
```

## 5. Booleans, Null



```yaml
active: true
inactive: false
value: null
```

Equivalent JSON:

```json
{
  "active": true,
  "inactive": false,
  "value": null
}
```

## 6. Complex Data Sructures: (List and Dictionaries)

```yaml
people:
  - name: Alice
    age: 28
  - name: Bob
    age: 34
```

Equivalent JSON:

```json
{
  "people": [
    { "name": "Alice", "age": 28 },
    { "name": "Bob", "age": 34 }
  ]
}
```

## 7. Separators:

``--`` used as a separators.

Often while defining a multiple resource creation in kubernetes yaml definition, ``--`` is used to separate different resources definitions.

```yaml
# First YAML document
name: Alice
age: 28

---
# Second YAML document
name: Bob
age: 32
job: Developer
```

## 8. Comments:

```yaml
# This is a comment
name: Alice  # This is an inline comment
age: 28
```

```yaml
# This is a comment
# that spans multiple lines
# in YAML format
name: Bob
age: 35
```

### Example:

```yaml
event:
  name: Annual Meeting
  date: "2024-12-21"
  location: Conference Room A
  attendees:
    - name: Alice
      role: Speaker
    - name: Bob
      role: Attendee
  vip:
    - name: Charlie
      role: VIP Speaker
  virtual: true
```

Equivalent JSON is:

```json
{
  "event": {
    "name": "Annual Meeting",
    "date": "2024-12-21",
    "location": "Conference Room A",
    "attendees": [
      { "name": "Alice", "role": "Speaker" },
      { "name": "Bob", "role": "Attendee" }
    ],
    "vip": [
      { "name": "Charlie", "role": "VIP Speaker" }
    ],
    "virtual": true
  }
}
```
