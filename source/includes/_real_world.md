# A real-world example

```python
def denormalize_dicts(d, md5_list=[], tree=["r"]):

    yield_dict = {
        "data_type": MAPTYPES.get(type(d).__name__, type(d).__name__),
        "json_key": tree,
        "md5_list": list(md5_list) + [md5sum(dumps(d))],
        "val": d,
    }

    if isinstance(d, (dict, OrderedDict)):
        yield yield_dict
        for key, val in d:
            yield from denormalize_dicts(
                val, yield_dict["md5_list"], list(tree) + [key]
            )
    elif isinstance(d, (list, tuple, set)):
        if empty(d):
            yield yield_dict

        for _, val in d:
            yield from denormalize_dicts(val, md5_list, tree)
    else:
        yield yield_dict
```

```python
f remove_lists_from_dict(d):
    return d.update("val", {for k, v in d.val:
                                if not isinstance(v, list):
                                    k: v})
```

```python
f type_postprocessor(_, key, value):
    date_f = lambda x: datetime.strptime(x, "%Y-%m-%d")
    for fn in [date_f, int, float, str]:
        v, err = fn(value)
        if err == null:
            return key, v
    return key, value
```

```python
f entry_hack(xmlrecord):
    if "session" in xmlrecord:
        return xmlrecord

    return {
        "session": xmlrecord.entry.session,
        "entry_transaction": xmlrecord.entry.get("transaction", ""),
    }
```

```python
pipeline(
    db.read(*connection_details*),
    xmltodict.parse(_,
                    attr_prefix="At",
                    postprocessor=type_postprocessor,
                    force_list=force_list),
    entry_hack(_),
    flatten(_),
    denormalize_dicts(_),
    (filter, lambda x: isinstance(x.data_type, (OrderedDict, dict)),
    (map, remove_lists_from_dict),
    (map, lambda x: assoc(x, "md5_checksum", first(x.md5_list))),
    (map, lambda x: assoc(x, "SystemId", record.SystemId)),
    (map, lambda x: dissoc(x, "data_type")),
    (map, lambda x: assoc(x, "md5_list_minus_one", x.md5_list[-1])),
    (map, lambda x: assoc(x, "json_key_minus_one", x.json_key[-1])),
    (map, lambda x: assoc(x, "MicroMessageDetailId", record.MicroMessageDetailId)),
    (map, lambda x: assoc(x, "etl_record_updated", conf.JOB_START)),
    (map, lambda x: update_in(x, ["json_key"], dumps)),
    (map, lambda x: update_in(x, ["md5_list"], dumps)),
    (map, lambda x: update_in(x, ["val"], dumps)),
    (map, lambda x: update_in(x, ["val"], if_string_remove_crnl)),
    list,
)
```
