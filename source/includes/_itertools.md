# Iteration tools

## Iterate through nested data

> Axon code

```python
nested = {"aaa": {"ccc": {"ggg": "acg",
                          "hhh": "ach"},
                  "ddd": {"iii": "adi",
                          "jjj": "adj"}},
          "bbb": {"eee": {"kkk": "bek",
                          "lll": "bel"},
                  "fff": {"mmm": "bfm",
                          "nnn": "bfn"}}}

for path, leaf in something(nested):
    print(path, leaf)
```

> Output

```bash
["aaa", "ccc", "ggg"], "acg"
["aaa", "ccc", "hhh"], "ach"
["aaa", "ddd", "iii"], "aci"
["aaa", "ddd", "jjj"], "acj"
["bbb", "eee", "kkk"], "bek"
["bbb", "eee", "lll"], "bel"
["bbb", "fff", "mmm"], "bfm"
["bbb", "fff", "nnn"], "bfn"
```

## Limiting by depth

```python
for path, leaf in something(nested, max_depth=2):
    print(path, leaf)
```

```bash
["aaa", "ccc"], {"ggg": "acg", "hhh": "ach"}
["aaa", "ddd"], {"iii": "adi", "jjj": "adj"}
["bbb", "eee"], {"kkk": "bek", "lll": "bel"}
["bbb", "fff"], {"mmm": "bfm", "nnn": "bfn"}
```

Mnemonic: A `max_depth` of `0` will be an iterable with a single element: the entire original data structure

## Limiting by filter

```python
f is_leaf(node):
    {k: _} = node

    if k in ["bbb", "ddd", "ggg", "hhh"]:
        return True
    else:
        return False

for path, leaf in something(nested, filter=is_leaf):
    print(path, leaf)
```

```bash
["aaa", "ccc", "ggg"], "acg"
["aaa", "ccc", "hhh"], "ach"
["aaa", "ddd"], {"iii": "adi", "jjj": "adj"}
["bbb"], {"eee": {"kkk": "bek", "lll": "bel"},
          "fff": {"mmm": "bfm", "nnn": "bfn"}}
```
