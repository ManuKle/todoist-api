# Reminders

> An example reminder object

```json
{
  "id": 12763422,
  "notify_uid": 1855589,
  "item_id": 33511505,
  "service": "email",
  "type": "absolute",
  "date_string": "Oct 6 @ 2pm",
  "date_lang" : "en",
  "due_date_utc": "Mon 06 Oct 2014 11:00:00 +0000",
  "due_date": "Mon 06 Oct 2014 11:00:00 +0000",
  "mm_offset": 180,
  "is_deleted": 0
}
```


Reminders are only available for Todoist Premium users.

### Properties

Property | Description
-------- | -----------
id | The id of the reminder (a unique number).
notify_uid | The user id which should be notified of the reminder, typically the current user id creating the reminder (a unique number).
item_id | The item id for which the reminder is about (a unique number).
service | The way to get notified of the reminder: `email` for e-mail, `mobile` for mobile text message, or `push` for mobile push notification.
type | The type of the reminder: `relative` for a time-based reminder specified in minutes from now, `absolute` for a time-based reminder with a specific time and date in the future, and `location` for a location-based reminder.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10` (or `null` or an empty string if not set). Look at our reference to see [which formats are supported](https://todoist.com/Help/DatesTimes).
date_lang | The language of the `date_string` (valid languages are: `en`, `da`, `pl`, `zh`, `ko`, `de`, `pt`, `ja`, `it`, `fr`, `sv`, `ru`, `es`, `nl`).
due_date_utc | The date of the task in a format like `Mon 07 Aug 2006 12:34:56 +0100` (or `null` if not set).
due_date | The same as `due_date_utc` (to be deprecated).
mm_offset | The relative time in minutes before the due date of the item, in which the reminder should be triggered. Note, that the item should have a due date set in order to add a relative reminder.
name | An alias name for the location (a string value).
loc_lat | The location latitude (a float number as string).
loc_long | The location longitude (a float number as a string).
loc_trigger | What should trigger the reminder: `on_enter` for entering the location, or `on_leave` for leaving the location.
radius | The radius around the location that is still considered as part of the location (a number in meters).
is_deleted | Whether the reminder is marked as deleted (where `1` is true and `0` is false).


## Add a reminder

> An example of adding a relative reminder:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_add", "temp_id": "e24ad822-a0df-4b7d-840f-83a5424a484a", "uuid": "41e59a76-3430-4e44-92b9-09d114be0d49", "args": {"item_id": 33511505, "service": "email", "minute_offset": 30}}]'
{ ...
  "SyncStatus": {"41e59a76-3430-4e44-92b9-09d114be0d49": "ok"},
  "TempIdMapping": {"e24ad822-a0df-4b7d-840f-83a5424a484a": 12763422},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.add(33511505, service='email', minute_offset=30)
>>> api.commit()
```

> An example of adding an absolute reminder:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_add", "temp_id": "952a365e-4965-4113-b4f4-80cdfcada172u", "uuid": "e7c8be2d-f484-4852-9422-a9984c58b1cd", "args": {"item_id": 33511505, "service": "email", "due_date_utc": "2014-10-15T11:00"}}]'
{ ...
  "SyncStatus": {"e7c8be2d-f484-4852-9422-a9984c58b1cd": "ok"},
  "TempIdMapping": {"952a365e-4965-4113-b4f4-80cdfcada172": 12763422},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.add(33511505, service='email', due_date_utc='2014-10-15T11:00')
>>> api.commit()
```

> An example of adding a location reminder:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_add", "temp_id": "7ad9609d-579f-4828-95c5-3600acdb2c81", "uuid": "830cf409-daba-479c-a624-68eb0c07d01c", "args": {"item_id": 33511505, "service": "email", "type": "location", "name": "Aliados", "loc_lat": "41.148581", "loc_long":"-8.610945000000015", "loc_trigger":"on_enter", "radius": 100}}]'
{ ...
  "SyncStatus": {"830cf409-daba-479c-a624-68eb0c07d01c": "ok"},
  "TempIdMapping": {"7ad9609d-579f-4828-95c5-3600acdb2c81": 12763422},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.add(33511505, service='email', type='location', name='Aliados', loc_lat='41.148581', loc_long='-8.610945000000015', loc_trigger='on_enter', radius=100)
>>> api.commit()
```

Add a reminder.

### Required arguments

Argument | Description
-------- | -----------
item_id | The item id for which the reminder is about (a number or temp id).

### Optional arguments

Argument | Description
-------- | -----------
notify_uid | The user id which should be notified of the reminder, typically the current user id creating the reminder (a unique number).
service | The way to get notified of the reminder: `email` for e-mail, `mobile` for mobile text message, or `push` for mobile push notification.
type | The type of the reminder: `relative` for a time-based reminder specified in minutes from now, `absolute` for a time-based reminder with a specific time and date in the future, and `location` for a location-based reminder.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10` (or `null` or an empty string to unset). Look at our reference to see [which formats are supported](https://todoist.com/Help/DatesTimes).
date_lang | The language of the `date_string` (valid languages are: `en`, `da`, `pl`, `zh`, `ko`, `de`, `pt`, `ja`, `it`, `fr`, `sv`, `ru`, `es`, `nl`).
due_date_utc | The date of the task in the format `YYYY-MM-DDTHH:MM` (for example: `2012-3-24T23:59`). The value of `due_date_utc` must be in UTC. Either `due_date_utc` or `date_string` can be used to set the reminder date, but the value of `due_date_utc` takes precedence over the value of `date_string`.
due_date | The same as `due_date_utc` (to be deprecated).
minute_offset | The relative time in minutes before the due date of the item, in which the reminder should be triggered. Note, that the item should have a due date set in order to add a relative reminder.
name | An alias name for the location (a string value).
loc_lat | The location latitude (a float number as string).
loc_long | The location longitude (a float number as a string).
loc_trigger | What should trigger the reminder: `on_enter` for entering the location, or `on_leave` for leaving the location.
radius | The radius around the location that is still considered as part of the location (a number in meters).

## Update a reminder

> An example of updating a reminder:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_update", "uuid": "b0e7562e-ea9f-4c84-87ee-9cbf9c103234", "args": {"id": 12763422, "due_date_utc": "2014-10-10T15:00"}}]'
{ ...
  "SyncStatus": {"b0e7562e-ea9f-4c84-87ee-9cbf9c103234": "ok"},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.get_by_id(12763422)
>>> reminder.update(due_date_utc='2014-10-10T15:00')
>>> api.commit()
```

Update a reminder.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the reminder (a number or temp id).

### Optional arguments

Argument | Description
-------- | -----------
notify_uid | The user id which should be notified of the reminder, typically the current user id creating the reminder (a unique number).
service | The way to get notified of the reminder: `email` for e-mail, `mobile` for mobile text message, or `push` for mobile push notification.
type | The type of the reminder: `relative` for a time-based reminder specified in minutes from now, `absolute` for a time-based reminder with a specific time and date in the future, and `location` for a location-based reminder.
date_string | The date of the task, added in free form text, for example it can be `every day @ 10` (or `null` or an empty string to unset). Look at our reference to see [which formats are supported](https://todoist.com/Help/DatesTimes).
date_lang | The language of the `date_string` (valid languages are: `en`, `da`, `pl`, `zh`, `ko`, `de`, `pt`, `ja`, `it`, `fr`, `sv`, `ru`, `es`, `nl`).
due_date_utc | The date of the task in the format `YYYY-MM-DDTHH:MM` (for example: `2012-3-24T23:59`). Either `due_date_utc` or `date_string` can be used to set the reminder date, but the value of `due_date_utc` takes precedence over the value of `date_string`.
due_date | The same as `due_date_utc` (to be deprecated).
minute_offset | The relative time in minutes before the due date of the item, in which the reminder should be triggered. Note, that the item should have a due date set in order to add a relative reminder.
name | An alias name for the location (a string value).
loc_lat | The location latitude (a float number as string).
loc_long | The location longitude (a float number as a string).
loc_trigger | What should trigger the reminder: `on_enter` for entering the location, or `on_leave` for leaving the location.
radius | The radius around the location that is still considered as part of the location (a number in meters).

## Delete a reminder

> An example of deleting a reminder:

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "reminder_delete", "uuid": "0896d03b-eb90-49f7-9020-5ed3fd09df2d", "args": {"id": 9}}]'
{ ...
  "SyncStatus": {"0896d03b-eb90-49f7-9020-5ed3fd09df2d": "ok"},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.reminders.get_by_id(12763422)
>>> reminder.delete()
>>> api.commit()
```

Delete a reminder.

### Required arguments

Argument | Description
-------- | -----------
id | The id of the filter (a number or temp id).


## Clear the locations

```shell
$ curl https://todoist.com/API/v6/sync \
    -d token=0123456789abcdef0123456789abcdef01234567 \
    -d commands='[{"type": "clear_locations", "uuid": "d285ae02-80c6-477c-bfa9-45272d7bddfb", "args": {}}]'
{ ...
  "SyncStatus": {"d285ae02-80c6-477c-bfa9-45272d7bddfb": "ok"},
  ... }
```

```python
>>> import todoist
>>> api = todoist.TodoistAPI('0123456789abcdef0123456789abcdef01234567')
>>> reminder = api.locations.clear()
>>> api.commit()
```

Clears the locations list, which is used for the location reminders.
