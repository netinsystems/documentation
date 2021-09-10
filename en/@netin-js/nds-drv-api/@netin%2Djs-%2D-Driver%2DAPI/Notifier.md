# Notifier

The entity `notifier`inside ***NetinDS DriverAPI***, allows you to report events and/or alarms about a NetinDS system.

Notification processes are treated as job requests, each call to one of the methods of the `notifier` returns a job ID, which can be used to identify the completion of the notification process by the event `done`.

For more information on the NetinDS alarm and notification system see the [documentation](https://docs.netin.io/netin_webUI/NetinDS/templates/).

## Table of Contents

*   [Notifier](#notifier)
    *   [Table of Contents](#tabla-de-contenidos)
    *   [Interface: **`Event`**](#interfaz-event)
    *   [Method: **`notify` (`severity: 1`, `cot: came`)**](#método-notify-severity-1-cot-came)
    *   [Method: **`inform` (`severity: 201`, `cot: came`)**](#método-inform-severity-201-cot-came)
    *   [Method: **`warn` (`severity: 401`, `cot: came`)**](#método-warn-severity-401-cot-came)
    *   [Method: **`error` (`severity: 601`, `cot: came`)**](#método-error-severity-601-cot-came)
    *   [Method: **`critical` (`severity: 801`, `cot: came`)**](#método-critical-severity-801-cot-came)
    *   [Method: **`deactivate` (`cot: went`)**](#método-deactivate-cot-went)

***

## Interface: **`Event`**

The interface `Event` defines an event from the driver's point of view and its relationship to the NetinDS data model.

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `origin` | `string` | | The id of the device or system that is the source of the event. <br> ***Format***: `/^[a-zA-Z0-9-_.]{1,80}$/`|
| `datapointSetId` | `string` | | The identifier of the datapointSet with which this event is associated. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/` |
| `datapointId` | `string` | | The identifier of the datapoint with which this event is associated. <br> ***Format***: `/^[a-zA-Z0-9#&_-]{1,80}$/` |
| `tableIndex` | `number` | `0` | `optional` For table-type datapointsets, the index of the table with which this event is associated |
| `text` | `string` | | Descriptive text about the event. <br> ***Limit***: `240` |
| `textHelp` | `string` | | Detailed information about the event or how to resolve it. <br> ***Limit***: `240` |
| `quality` | [`Quality`](https://docs.netin.io/) | `good` | `optional` Quality of the alarm or datapoint on which it is based, in other words, how reliable this alarm is or the value of the datapoint |
| `qualityCode` | [`QualityType`](https://docs.netin.io/) | `good` | `optional` Type of alarm quality or datapoint, in other words, the reason for the current value of `quality` |
| `timestamp` | `number` | `Date.now()` | `optional` Timestamp in epoch format in milliseconds |
| `codes` | `string[]` | `[]` | `optional` Event identification codes. They are used for the differentiation of different events in cases of multi-instance, that is, when the same `origin`, `datapointSetId` and `datapointId` they can have different types of events active at the same time |
| `facility` | `number` | `-1` | `optional` Identification of the resource group to which the event belongs. <br> ***Limit***: `255` |

***

## Method: **`notify` (`severity: 1`, `cot: came`)**

Create a notification job for a level event **notification** which will be transmitted to NetinDS, the work ID is returned when it is created. when the job resolves that is, when the event is reported to the netinds system the event is issued [`done`](./Driver.md#evento-done) along with the job ID.

```typescript
notify(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *String*) => *void* | Optional callback function |

    **Returns:** `void`

```typescript
notify(event: Event): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |

    **Returns:** `Promise<string>`

***

## Method: **`inform` (`severity: 201`, `cot: came`)**

Create a notification job for a level event **informative** which will be transmitted to NetinDS, the work ID is returned when it is created. when the job resolves that is, when the event is reported to the netinds system the event is issued [`done`](./Driver.md#evento-done) along with the job ID.

```typescript
inform(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *String*) => *void* | Optional callback function |

    **Returns:** `void`

```typescript
inform(event: Event): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |

    **Returns:** `Promise<string>`

***

## Method: **`warn` (`severity: 401`, `cot: came`)**

Create a notification job for a level event **warning** which will be transmitted to NetinDS, the work ID is returned when it is created. when the job resolves that is, when the event is reported to the netinds system the event is issued [`done`](./Driver.md#evento-done) along with the job ID.

```typescript
warn(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *String*) => *void* | Optional callback function |

    **Returns:** `void`

```typescript
warn(event: Event): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |

    **Returns:** `Promise<string>`

## Method: **`error` (`severity: 601`, `cot: came`)**

Create a notification job for a level event **error** which will be transmitted to NetinDS, the work ID is returned when it is created. when the job resolves that is, when the event is reported to the netinds system the event is issued [`done`](./Driver.md#evento-done) along with the job ID.

```typescript
error(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *String*) => *void* | Optional callback function |

    **Returns:** `void`

```typescript
error(event: Event): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |

    **Returns:** `Promise<string>`

***

## Method: **`critical` (`severity: 801`, `cot: came`)**

Create a notification job for a level event **Critical** which will be transmitted to NetinDS, the work ID is returned when it is created. when the job resolves that is, when the event is reported to the netinds system the event is issued [`done`](./Driver.md#evento-done) along with the job ID.

```typescript
critical(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *String*) => *void* | Optional callback function |

    **Returns:** `void`

```typescript
critical(event: Event): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | Event to publish to NetinDS |

    **Returns:** `Promise<string>`

***

## Method: **`deactivate` (`cot: went`)**

Create a notification job for the **depublication** which will be transmitted to NetinDS, the work ID is returned when it is created. when the job resolves that is, when the event is reported to the netinds system the event is issued [`done`](./Driver.md#evento-done) along with the job ID.

```typescript
deactivate(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | NetinDS | unpublished event
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *String*) => *void* | Optional callback function |

    **Returns:** `void`

```typescript
deactivate(event: Event): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `event` | [*Event*](#interfaz-event) | NetinDS | unpublished event

    **Returns:** `Promise<string>`
