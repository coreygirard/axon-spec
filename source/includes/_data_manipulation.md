# Data manipulation

> Input data

```json
{
  "policy type": "something",
  "policy data": {
    "stuff": [123, 456, 789],
    "other": "test"
  },
  "holder": {
    "name": "John Smith",
    "address": ["123 Main St", "Somewhere, NY, USA"]
  }
}
```

> Input spec

```json
{
  "policy_type": ".policy type",
  "policy_data": ".policy data",
  "holder_name": ".holder.name",
  "holder_address": ".holder.address"
}
```

> Output spec

```json
{
  "name": ".holder_name",
  "address": ".holder_address",
  "policy": {
    "type": ".policy_type",
    "data": ".policy_data"
  }
}
```

> Axon program

```python
m = something_extract(data, input_spec)
something_build(m, output_spec)
```

> Output data

```json
{
  "name": "John Smith",
  "address": ["123 Main St", "Somewhere, NY, USA"],
  "policy": {
    "type": "something",
    "data": {
      "stuff": [123, 456, 789],
      "other": "test"
    }
  }
}
```
