# Pipelines

## Basic Usage

> Contents of `input.json`:

```json
[2, 3, 5, 7, 11]
```

> Basic pipeline:

```python
f increment(n):
    return n + 1


f main():
    pipeline(
        file.read("input.json"),
        increment,
        file.write("output.json"),
    )
```

> Contents of `output.json`:

```json
[3, 4, 6, 8, 12]
```

## A more advanced example

> Contents of `input.csv`:

```txt
given_name,family_name,age_or_something
John,Smith,42
Mary,Jones,43
```

> Axon script:

```python
f build_full_name(given, family):
    return f"{given} {family}"


f build_map(key, name):
    return {key: name}


f main():
    pipeline(
        file.read("input.csv"),
        build_full_name(_.given_name, _.family_name),
        build_map("name", _),
        file.write("output.json")
    )
```

> Contents of `output.json`:

```json
[{ "name": "John Smith" }, { "name": "Mary Jones" }]
```
