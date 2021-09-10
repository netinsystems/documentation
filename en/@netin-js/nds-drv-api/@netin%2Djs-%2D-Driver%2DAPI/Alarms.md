# Alarms

The entity `alarms`inside ***NetinDS DriverAPI***, allows access to the repository of alarms/events currently active on the DriverAPI instance.

## Table of Contents

*   [Alarms](#alarms)
    *   [Table of Contents](#tabla-de-contenidos)
    *   [Method: **`getAlarm`**](#método-getalarm)
    *   [Method: **`getAlarms`**](#método-getalarms)
    *   [Method: **`getByOrigin`**](#método-getbyorigin)
    *   [Method: **`hasAlarm`**](#método-hasalarm)

***

## Method: **`getAlarm`**

Returns an alarm/event if it is present in the repository

```typescript
getAlarm(alarmId: string): undefined | Event
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `alarmId` | *String* | Alarm/event requested |

    **Returns:** [`Event`](./Notifier.md#interfaz-event) | `undefined`

***

## Method: **`getAlarms`**

Returns all alarms/events present in the repository

```typescript
getAlarms(): Event[][]
```

**Returns:** [`Event[][]`](./Notifier.md#interfaz-event) | `undefined`

***

## Method: **`getByOrigin`**

Returns all alarms/events present in the repository for a source.

```typescript
getByOrigin(origin: string): undefined | Event[]
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `origin` | *String* | Requested source |

    **Returns:** [`Event[]`](./Notifier.md#interfaz-event) | `undefined`

***

## Method: **`hasAlarm`**

Returns a positive result if the alarm/event is present in the repository.

```typescript
hasAlarm(alarmId: string): boolean
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `alarmId` | *String* | Alarm/event requested |

    **Returns:** `boolean`
