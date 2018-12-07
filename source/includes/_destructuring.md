# Destructuring

The overall guiding concept for Axon's destructuring is _"make the expression on the left evaluate to the same value as the expression on the right"_. There are places where this principle is sacrificed in favor of being concise and/or powerful, but it's a good starting point to keep in mind.

## Destructuring lists

### List of N elements -> N variables

```python
data = [1, 2, 3]

a, b, c = data

assert a == 1
assert b == 2
assert c == 3
```

The most basic example of destructuring is assigning from a list to multiple variables at once.

<br><br><br><br>

### Split head from list

```python
data = [1, 2, 3]

a, *b = data

assert a == 1
assert b == [2, 3]
```

We are able to assign everything following the first element into `b` by marking it with an asterisk.

<br><br><br><br>

### Split tail from list

```python
data = [1, 2, 3]

*a, b = data

assert a == [1, 2]
assert b == 3
```

When we use the same notation on `a` instead, it receives all elements except the final one.

<br><br><br><br>

### More advanced examples

```python
data = [1, 2, 3, 4, 5]

a, *b, c = data
assert a == 1
assert b == [2, 3, 4]
assert c == 5

a, b, *c = data
assert a == 1
assert b == 2
assert c == [3, 4, 5]

a, b, *c, d = data
assert a == 1
assert b == 2
assert c == [3, 4]
assert d == 5
```

We aren't limited to assigning only into two variables

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

```python
data = [1, 2, 3, 4, 5]

*a, b, *c = data  # error, because partitions are indeterminate
```

The only limitation is that we can't have multiple asterisks, because Axon won't know how to divide elements between them.

## Destructuring maps

```python
m = {"key": "value"}

{"key": v} = m

assert v == "value"
```

The same principle of making the left side equal to the right holds here.

<br><br><br><br><br><br>

```python
m = {"key": "value"}

{k: "value"} = m

assert k == "key"
```

TODO: should this work? More powerful and concise, but how to handle finding zero or multiple keys with a given value? Probably tends to lead to sloppy error handling, so we shouldn't include it.

<br><br><br><br>

```python
m = {"key1": "value1", "key2": "value2"}

{"key1": v} = m

assert v == "value1"
```

Here's a small exception for the sake of power. We are able to select only a subset of keys, and therefore values. In this case, the left expression evaluates to  
`{"key1": "value1"}`, while the right expression evaluates to  
`{"key1": "value1", "key2": "value2"}`.

<br><br><br><br>

```python
data = {"key": "value"}

{k: _} = data
assert k == "key"

{_: v} = data
assert v == "value"

{k: v} = data
assert k == "key"
assert v == "value"
```

_When there is only one key-value pair_, we may select them via this notation.

Note: `{_: _}` is a syntax error, as it would simply produce the same map.

<br><br><br><br>

## Destructuring strings

```python
e = "john.smith@mail.com"

first, ".", last, "@", domain = e

assert first == "john"
assert last == "smith"
assert domain == "mail.com"
```

## Nesting

```python
data = {"key1": [1, 2, 3, 4],
        "key2": [1, 2, 3]}

{"key1": a, b, *c,
 "key2": d, e, *f} = data

# now a == 1, b == 2, c == [3, 4],
# and d == 1, e == 2, f == [3]
```

```python
data = [1, {"some": "values", "other": "things"}, 2, 3, 4]

a, {"some": v}, *b, c = data

# now a == 1, v == "values", b == [2, 3], c == 4
```
