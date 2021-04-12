## `move` operator

The `move` operator moves (or renames) a field from one location to another.

### Configuration Fields

| Field      | Default          | Description                                                                                                                                                                                                                              |
| ---        | ---              | ---                                                                                                                                                                                                                                      |
| `id`       | `move`    | A unique identifier for the operator                                                                                                                                                                                                     |
| `output`   | Next in pipeline | The connected operator(s) that will receive all outbound entries                                                                                                                                                                         |
| `from`      | required       | The [field](/docs/types/field.md)  to move the value out of.   
| `to`      | required       | The [field](/docs/types/field.md)  to move the value into.
| `on_error` | `send`           | The behavior of the operator if it encounters an error. See [on_error](/docs/types/on_error.md)                                                                                                                                          |
| `if`       |                  | An [expression](/docs/types/expression.md) that, when set, will be evaluated to determine whether this operator should be used for the given entry. This allows you to do easy conditional parsing without branching logic with routers. |

Example usage:

Rename value
```yaml
- type: move
    from: key1
    to: key3
```

<table>
<tr><td> Input Entry</td> <td> Output Entry </td></tr>
<tr>
<td>

```json
{
  "resource": { },
  "attributes": { },  
  "body": {
    "key1": "val1",
    "key2": "val2"
  }
}
```

</td>
<td>

```json
{
  "resource": { },
  "attributes": { },  
  "body": {
    "key3": "val1",
    "key2": "val2"
  }
}
```

</td>
</tr>
</table>
<hr>

Move a value from the body to resource

```yaml
- type: move
    from: uuid
    to: $resoruce.uuid
```

<table>
<tr><td> Input Entry</td> <td> Output Entry </td></tr>
<tr>
<td>

```json
{
  "resource": { },
  "attributes": { },  
  "body": {
    "uuid": "091edc50-d91a-460d-83cd-089a62937738"
  }
}
```

</td>
<td>

```json
{
  "resource": { 
    "uuid": "091edc50-d91a-460d-83cd-089a62937738"
  },
  "attributes": { },  
  "body": { }
}
```

</td>
</tr>
</table>

<hr>

Move a value from the body to attributes

```yaml
- type: move
    from: ip
    to: $attributes.ip
```

<table>
<tr><td> Input Entry</td> <td> Output Entry </td></tr>
<tr>
<td>

```json
{
  "resource": { },
  "attributes": { },  
  "body": {
    "ip": "8.8.8.8"
  }
}
```

</td>
<td>

```json
{
  "resource": { },
  "attributes": { 
    "ip": "8.8.8.8"
  },  
  "body": { }
}
```

</td>
</tr>
</table>

<hr>

Replace the body with an individual value nested within the body
```yaml
- type: move
    from: log
    to: $body
```

<table>
<tr><td> Input Entry</td> <td> Output Entry </td></tr>
<tr>
<td>

```json
{
  "resource": { },
  "attributes": { },  
  "body": {
    "log": "The log line"
  }
}
```

</td>
<td>

```json
{
  "resource": { },
  "attributes": { },  
  "body": "The log line"
}
```

</td>
</tr>
</table>

<hr>

Remove a layer from the body
```yaml
- type: move
    from: wrapper
    to: $body
```

<table>
<tr><td> Input Entry</td> <td> Output Entry </td></tr>
<tr>
<td>

```json
{
  "resource": { },
  "attributes": { },  
  "body": {
    "wrapper": {
      "key1": "val1",
      "key2": "val2",
      "key3": "val3"
    }
  }
}
}
```

</td>
<td>

```json
{
  "resource": { },
  "attributes": { },  
  "body": {
    "key1": "val1",
    "key2": "val2",
    "key3": "val3"
  }
}
```

</td>
</tr>
</table>
