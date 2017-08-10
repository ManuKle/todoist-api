# Filters

> An example filter object

```json
{
  "id": 4638878,
  "user_id": 1855589,
  "name": "Priority 1",
  "query": "priority 1",
  "color": 6,
  "item_order": 3,
  "is_deleted": 0
}
```

Filters are only available for Todoist Premium users.

### Properties

Property | Description
-------- | -----------
id | The id of the filter (a unique number).
name | The name of the filter (a string value).
query | The query to search for. [Examples of searches](https://todoist.com/Help/Filtering) can be found in the Todoist help page.
color | The color of the filter (a number between `0` and `7`, or between `0` and `12` for premium users).  The color codes corresponding to these numbers are: `#019412`, `#a39d01`, `#e73d02`, `#e702a4`, `#9902e7`, `#1d02e7`, `#0082c5`, `#555555`.  And for the additional colors of the premium users: `#008299`, `#03b3b2`, `#ac193d`, `#82ba00`, `#111111`.
item_order | Filter’s order in the filter list (a number, where the smallest value should place the filter at the top).
is_deleted | Whether the filter is marked as deleted (where `1` is true and `0` is false).

## Add a filter

> An example of adding a filter:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "filter_add", "temp_id": "9204ca9f-e91c-436b-b408-ea02b3972686", "uuid": "0b8690b8-59e6-4d5b-9c08-6b4f1e8e0eb8", "args": {"name": "Filter1", "query": "no due date"}}]'
{ ...
  "SyncStatus": {"0b8690b8-59e6-4d5b-9c08-6b4f1e8e0eb8": "ok"},
  "TempIdMapping": {"9204ca9f-e91c-436b-b408-ea02b3972686": 9},
  ... }

```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filters.add('Filter1', 'no due date')
>>> api.commit()
```

Add a filter.

### Required arguments

Argument | Description
-------- | -----------
name | The name of the filter (a string value).
query | The query to search for. [Examples of searches](https://todoist.com/Help/Filtering) can be found in the Todoist help page.

### Optional arguments

Argument | Description
-------- | -----------
color | The color of the filter (a number between `0` and `7`, or between `0` and `12` for premium users).
item_order | Filter’s order in the filter list (a number, where the smallest value should place the filter at the top).

## Update a filter

> An example of updating a filter:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "filter_update", "uuid": "a68b588a-44f7-434c-b3c5-a699949f755c", "args": {"id": 9, "query": "tomorrow"}}]'
{ ...
  "SyncStatus": {"a68b588a-44f7-434c-b3c5-a699949f755c": "ok"},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filters.get_by_id(9)
>>> filter.update(query='tomorrow')
>>> api.commit()
```

Update a filter.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the filter (a number or temp id).

### Optional arguments

Argument | Description
-------- | -----------
name | The name of the filter (a string value).
query | The query to search for. [Examples of searches](https://todoist.com/Help/Filtering) can be found in the Todoist help page.
color | The color of the filter (a number between `0` and `7`, or between `0` and `12` for premium users).
item_order | Filter’s order in the filter list (a number, where the smallest value should place the filter at the top).

## Delete a filter

> An example of deleting a filter:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "filter_delete", "uuid": "b8186025-66d5-4eae-b0dd-befa541abbed", "args": {"id": 9}}]'
{ ...
  "SyncStatus": {"b8186025-66d5-4eae-b0dd-befa541abbed": "ok"},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> filter = api.filters.get_by_id(9)
>>> filter.delete()
>>> api.commit()
```

Delete a filter.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the filter (a number or temp id).

## Update multiple orders

> An example of updating the orders of multiple filters at once:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands=[{"type": "filter_update_orders", "uuid": "517560cc-f165-4ff6-947b-3adda8aef744", "args": {"id_order_mapping": {"9":  1, "10": 2}}}]'
{ ...
  "SyncStatus": {
    "517560cc-f165-4ff6-947b-3adda8aef744": {
      "9": "ok",
      "10": "ok"
    }
  },
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> api.filters.update_orders({9: 1, 10: 2})
>>> api.commit()
```

Update the orders of multiple filters at once.

### Required arguments

Argument | Description
-------- | -----------
id_order_mapping| A dictionary, where a filter id is the key, and the order its value: `filter_id: order`.
