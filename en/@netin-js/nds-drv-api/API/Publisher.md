# Publisher

The entity `publisher`inside ***NetinDS DriverAPI***, allows you to publish information about a NetinDS system, in the different formats supported: time series and datapointSet of type map / table.

Publishing processes are treated as job requests, each call to one of the methods of the `publisher` returns a job ID, which can be used to identify the completion of the publishing process by the event `done`.

For more information on the NetinDS data model see the [documentation](https://docs.netin.io/).

## Table of Contents

*   [Publisher](#publisher)
    *   [Table of Contents](#tabla-de-contenidos)
    *   [Interface: **`DataSetMap`(*map*)**](#interfaz-datasetmapmap)
    *   [Interface: **`DataSetBulk`(*table*)**](#interfaz-datasetbulktable)
    *   [Interface: **`DataSetEntry`(*Datapoint*)**](#interfaz-datasetentrydatapoint)
    *   [Method: **`publish`** (*maps*)](#método-publish-maps)
    *   [Method: **`publishBulk`** (*tables*)](#método-publishbulk-tables)

***

## Interface: **`DataSetMap`(*map*)**

The interface `DataSetMap` defines a dataset, within the context of NetinDS, with a map format (*Key*).

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `origin` | `string` | | Identifier of the device or system that is the source of the information. <br> ***Format***: `/^[a-zA-Z0-9-_.]{1,80}$/`|
| `datapointSetId` | `string` | | The identifier of the datapointSet with which the information is associated. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/` |
| `quality` | [`Quality`](https://docs.netin.io/) | `good` | `optional` Quality of the alarm or datapoint on which it is based, in other words, how reliable this alarm is or the value of the datapoint |
| `qualityCode` | [`QualityType`](https://docs.netin.io/) | `good` | `optional` Type of alarm quality or datapoint, in other words, the reason for the current value of `quality` |
| `timestamp` | `number` | `Date.now()` | `optional` Timestamp in epoch format in milliseconds |
| `data` | [`DataSetEntry[]`](#interfaz-datasetentry_datapoint\_) | | Information to be published in the NetinDS system |

***

## Interface: **`DataSetBulk`(*table*)**

The interface `DataSetBulk` defines a dataset, within the context of NetinDS, with a table format, or what is the same, an array of maps (*Key*).

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `origin` | `string` | | Identifier of the device or system that is the source of the information. <br> ***Format***: `/^[a-zA-Z0-9-_.]{1,80}$/`|
| `datapointSetId` | `string` | | The identifier of the datapointSet with which the information is associated. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/` |
| `quality` | [`Quality`](https://docs.netin.io/) | `good` | `optional` Quality of the alarm or datapoint on which it is based, in other words, how reliable this alarm is or the value of the datapoint |
| `qualityCode` | [`QualityType`](https://docs.netin.io/) | `good` | `optional` Type of alarm quality or datapoint, in other words, the reason for the current value of `quality` |
| `timestamp` | `number` | `Date.now()` | `optional` Timestamp in epoch format in milliseconds |
| `data` | [`DataSetEntry[][]`](#interfaz-datasetentry_datapoint\_) | | Information to be published in the NetinDS system |

***

## Interface: **`DataSetEntry`(*Datapoint*)**

The interface `DataSetEntry` defines the minimum unit of information of a netinds system, that is, a `datapoint`. They represent a real plane value (measurements, sensor status, setpoints...), logical (logical state of a port, state of a process, ...) or adjacent to the system (meta information about devices, facilities, field elements...).

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `datapointId` | `string` | | The identifier of the datapoint with which this value is associated. <br> ***Format***: `/^[a-zA-Z0-9#&_-]{1,80}$/` |
| `value` | `any` | | `optional` Value treated and adapted to the user. |
| `rawValue` | `any` | | `optional` Original value, as collected from the source. |
| `quality` | [`Quality`](https://docs.netin.io/) | `good` | `optional` Data quality, in other words, how reliable is this value |
| `qualityCode` | [`QualityType`](https://docs.netin.io/) | `good` | `optional` Type of data quality, in other words, the reason for the current value of `quality` |
| `timestamp` | `number` | `Date.now()` | `optional` Timestamp in epoch format in milliseconds |
| `meta` | `string` | | `optional` Any type of information related to the value in text format |

> While the fields `value`and `rawValue`are both optional, at least one of the two must be indicated.

***

## Method: **`publish`** (*maps*)

Create a publishing job for a **DataSetMap** or a set of **DataSetMap** which will be transmitted to NetinDS, the work ID is returned when it is created. When resolving the work, that is, when the information has been transmitted to the NetinDS system, the event is issued [`done`](./Driver.md#evento-done) along with the job ID.

```typescript
publish(origin: string, datapointSetId: string, data: Record<string, unknown>): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `origin` | *String* | Identifier of the device or system that is the source of the information. <br> ***Format***: `/^[a-zA-Z0-9-_.]{1,80}$/`|
    | `datapointSetId` | *String* | The identifier of the datapointset with which this information is associated. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/`|
    | `data` | *Record*\<string, unknown> | Information to be published to the NetinDS system in format *Key*. The name of each property will be used as `datapointId` and the value of this property as `rawValue` in a type structure [`DataEntry`](#interfaz-datasetentry_datapoint\_). |

    **Returns:** `Promise<string>`

```typescript
publish(origin: string, datapointSetId: string, data: Record<string, unknown>, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `origin` | *String* | Identifier of the device or system that is the source of the information. |
    | `datapointSetId` | *String* | The identifier of the datapointset with which this information is associated. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/`|
    | `data` | *Record*\<string, unknown> | Information to be published to the NetinDS system in format *Key*. The name of each property will be used as `datapointId` and the value of this property as `rawValue` in a type structure [`DataEntry`](#interfaz-datasetentry_datapoint\_). |
    | `callback` | (`error?`: *Error*, `eventId?`: *String*) => *void* | Optional callback function. |

    **Returns:** `void`

```typescript
publish(data: DataSetMap | DataSetMap[]): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `data` | [`DataSetMap`](#interfaz-datasetmap_map\_) | [`DataSetMap[]`](#interfaz-datasetmap_map\_) | Information to be published in the NetinDS system. |

    **Returns:** `Promise<string>`

```typescript
publish(data: DataSetMap | DataSetMap[], callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `data` | [`DataSetMap`](#interfaz-datasetmap_map\_) | [`DataSetMap[]`](#interfaz-datasetmap_map\_) | Information to be published in the NetinDS system. |
    | `callback` | (`error?`: *Error*, `eventId?`: *String*) => *void* | Optional callback function. |

    **Returns:** `void`

***

## Method: **`publishBulk`** (*tables*)

Create a publishing job for a **DataSetBulk** or a set of **DataSetBulk** which will be transmitted to NetinDS, the work ID is returned when it is created. When resolving the work, that is, when the information has been transmitted to the NetinDS system, the event is issued [`done`](./Driver.md#evento-done) along with the job ID.

```typescript
publishBulk(data: DataSetBulk | DataSetBulk[]): Promise<string>
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `data` | [`DataSetBulk`](#interfaz-datasetbulk_table\_) | [`DataSetBulk[]`](#interfaz-datasetbulk_table\_) | Information to be published in the NetinDS system |

    **Returns:** `Promise<string>`

```typescript
publishBulk(data: DataSetBulk | DataSetBulk[], callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `data` | [`DataSetBulk`](#interfaz-datasetbulk_table\_) | [`DataSetBulk[]`](#interfaz-datasetbulk_table\_) | Information to be published in the NetinDS system |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *String*) => *void* | Optional callback function |

    **Returns:** `void`
