# Pipelines

Axon's `pipeline` functionality provides a clean syntax for specifying data engineering pipelines without having to manually control flow.

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
        increment(_),
        file.write(_, "output.json"),
    )
```

> Contents of `output.json`:

```json
[3, 4, 6, 8, 12]
```

Placeholder

<br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br><br>

```python
file.read("input.json"),
```

The first argument to a pipeline must generate a stream. Usually a file reader or a database query. For example, a CSV reader will stream one line at a time, a JSON reader will stream one object at a time if the global structure is a list, otherwise a stream with a single element, which is the entire file. A database query will stream one row at a time. Etc.

```python
increment(_),
```

All arguments after the first require an `_` argument in some position. This specifies the position for the elements of the stream to be placed when calling the function. If the function takes only a single argument, the `(_)` may be dropped. IE `increment(_)` and `increment` are equivalent in the context of a pipeline.

```python
file.write(_, "output.json"),
```

This is an example of using the `_` argument to specify order of arguments. The `file.write` function takes two arguments. The first is a stream to be written to file, and the second is the filepath. Since our stream is what we want written, we place the `_` in the first location. Though this is the most common way to use the `file.write` function, we could imagine wanting to write a string to a number of different files. In that case, we would do something like `file.write("hello world", _)`

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
