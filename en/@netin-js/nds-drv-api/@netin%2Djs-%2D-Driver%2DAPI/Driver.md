# Driver

The entity `Driver`, is the principal of the  ***NetinDS DriverAPI***, allowing you to create a new instance of it. This entity is the primary access point to all other methods, properties, and entities.

## Table of Contents

*   [Driver](#driver)
    *   [Table of Contents](#tabla-de-contenidos)
    *   [Interface: **`DriverOptions`**](#interfaz-driveroptions)
    *   [Interface: **`DriverHealthStatus`**](#interfaz-driverhealthstatus)
    *   [Interface: **`ComponentDetails`**](#interfaz-componentdetails)
    *   [Interface: **`SubcomponentDetail`**](#interfaz-subcomponentdetail)
    *   [Interface: **`JobResult`**](#interfaz-jobresult)
    *   [Interface: **`DriverFailure`**](#interfaz-driverfailure)
    *   [Enum: **`SubcomponentState`**](#enum-subcomponentstate)
    *   [Constructor: **`Driver`**](#constructor-driver)
    *   [Property: **`addresses`**](#propiedad-addresses)
    *   [Property: **`alarms`**](#propiedad-alarms)
    *   [Property: **`notifier`**](#propiedad-notifier)
    *   [Property: **`publisher`**](#propiedad-publisher)
    *   [Property: **`configErrors`**](#propiedad-configerrors)
    *   [Property: **`configState`**](#propiedad-configstate)
    *   [Property: **`hasConfigErrors`**](#propiedad-hasconfigerrors)
    *   [Property: **`id`**](#propiedad-id)
    *   [Property: **`isEnrolled`**](#propiedad-isenrolled)
    *   [Property: **`state`**](#propiedad-state)
    *   [Property: **`status`**](#propiedad-status)
    *   [Event: **`done`**](#evento-done)
    *   [Event: **`update`**](#evento-update)
    *   [Event: **`updated`**](#evento-updated)
    *   [Event: **`error`**](#evento-error)
    *   [Event: **`state`**](#evento-state)
    *   [Method: **`addFailure`**](#método-addfailure)
    *   [Method: **`update`**](#método-update)
    *   [Method: **`updateSubcomponent`**](#método-updatesubcomponent)
    *   [Method: **`disenroll`**](#método-disenroll)
    *   [Method: **`enroll`**](#método-enroll)

***

## Interface: **`DriverOptions`**

The interface `DriverOptions` defines the configuration options for the instance of **DriverAPI**.

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `artifactId` | `string` | | Driver ID, must match the value of the property `originType` in the `addressConfig` of the *datapoints* assigned to this driver and with the driver configuration entry in the property `originTypes` of the [*templates*](https://docs.netin.io/netin_webUI/NetinDS/templates/). |
| `updateInterval` | `number` | `60000` | `optional` The time interval between requests to verify new configuration. |
| `autoUpdate` | `boolean` | `false` | `optional` Automatic update, if the driver is activated it will not issue events `update`, but will proceed to update the configuration as soon as it is available, issuing the events `updated` subsequently.  |
| `healthPort` | `number` | `3000` | `optional` Port where **DriverAPI** must provide the observability interface of **NetinDS** |
| `agentHostAddress` | `string` | `localhost`| `optional` Host where the **NetinDS Agent** |
| `discoveryHostAddress` | `string` | `localhost`| `optional` Host where the service is located **Discovery** of the **Netin Agent**. If configured, it will be used instead of `agentHostAddress` |
| `rdbHostAddress` | `string` | `localhost`| `optional` Host where the service is located **RDB** of the **Netin Agent**. If configured, it will be used instead of `agentHostAddress` |
| `semVer` | `string` | `0.0.0`| `optional` Version of the driver release in semver format |

***

## Interface: **`DriverHealthStatus`**

The interface `DriverHealthStatus` indicates the relevant information about the status of the **DriverAPI**.

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `status` | `'pass' \| 'fail' \| 'warn'` | | Indicates the *status* of **DriverAPI** from the point of view of the *observability* of **NetinDS**. |
| `description` | `string` | | `optional` Description of the instance of the **DriverAPI**. |
| `releaseID` | `string` | | `optional` Release version, in semver format. Matches the value of the property `semVer`of the interface [`DriverOptions`](#interfaz-driveroptions) indicated in the initialization of the driver or environment variable `CONFIG_ARTIFACT_RELEASE`, otherwise indicates the release of the **DriverAPI**. |
| `version` | `string` | | `optional` Driver version, inferred from property value `semVer`of the interface [`DriverOptions`](#interfaz-driveroptions) indicated in the initialization of the driver or environment variable `CONFIG_ARTIFACT_RELEASE`, otherwise indicates the version of the **DriverAPI**. |
| `serviceId` | `string` | | Driver *Universally Unique Identifier* (*UUID*), matches the value of [`id`](#propiedad-id). |
| `output` | `string` | | `optional` Extra information about the current status of the **DriverAPI**. |
| `notes` | `string[]` | | `optional` Array with relevant notes on the current state of the driver **DriverAPI**. |
| `links` | `object` | | `optional` Object with external links with information of interest for the current state of the **DriverAPI**. |
| `details`| [`ComponentDetails`](#interfaz-componentdetails) | | `optional` Object with the representation of the *status* of the different subcomponents that form the instance of the **DriverAPI**. |

```json
{
    status: "pass",
    version: "1",
    releaseID: "1.0.1",
    notes: [ ],
    output: "",
    serviceID: "c76fc8c4-ec5f-4365-88ff-e38883dfa937",
    description: "Driver-API Health for netin-ds-drv-mqtt",
    links: {
        about: "https://docs.netin.io"
    },
    details: {
        addressesLegacy:connectionState: [
            {
                status: "pass",
                componentId: "abc99c74-c59c-4014-a1dc-1acb459c88e6",
                componentType: "datastore",
                metricValue: "running",
                time: "2021-05-16T10:56:02.578Z"
            }
        ],
        redis:connectionState: [
            {
                status: "pass",
                componentId: "757f7422-3512-4524-ad3e-bf45620caed8",
                componentType: "datastore",
                metricValue: "running",
                time: "2021-05-04T14:33:59.757Z"
            }
        ],
        config:state: [
            {
                status: "pass",
                componentId: "4781a874-269d-4c31-b9fa-1bca3bb48cd8",
                componentType: "component",
                metricValue: "running",
                time: "2021-05-04T14:33:16.405Z"
            }
        ],
        curator:component: [
            {
                status: "pass",
                componentId: "36fd2af5-3605-4245-8557-0888dfc7eff6",
                componentType: "component",
                metricValue: 200,
                metricUnit: "jobs",
                time: "2021-05-16T10:56:22.741Z"
            }
        ],
        publisher:component: [
            {
                status: "pass",
                componentId: "cf2faa54-83e2-4206-b9fb-a9ae5436b8bc",
                componentType: "component",
                metricValue: 100,
                metricUnit: "jobs",
                time: "2021-05-16T10:56:22.741Z"
            }
        ],
        netin-ds-drv-mqtt:external: [ ],
        uptime: [
            {
                componentId: "netin-ds-drv-mqtt",
                componentType: "system",
                metricValue: 1023791.194660235,
                metricUnit: "s",
                status: "pass",
                time: "2021-05-16T10:56:22.749Z"
            }
        ]
    }
}
```

***

## Interface: **`ComponentDetails`**

The interface `ComponentDetails` indicates the list of subcomponents that are part of the instance of the **DriverAPI**.

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `[subcomponent: string]` | `SubcomponentDetail[]` | | Each subcomponent can have its own input within this object. each entry can consist of one or more objects of type [`SubcomponentDetail`](#interfaz-subcomponentdetail). |

By default, any instance of **Driver-API** it shall have the following subcomponents:

*   `address:connectionState`: indicates the status of the connection against the configuration management system of *Origins*. This subcomponent will not be present if the subcomponent `addressLegacy:connectionState` is in operation.
*   `addressLegacy:connectionState`: indicates the status of the connection against the system *Legacy* configuration management *Origins*. This subcomponent will not be present if the subcomponent `address:connectionState` is in operation.
*   `redis:connectionState`: indicates the *status* of the connection to the Redis instance of the **NetinDS Agent**.
*   `config:state`: indicates the *status* of the instance configuration **DriverAPI**.
*   `curator:component`: indicates the *status* of the subcomponent that analyzes the publication and notification jobs to avoid errors in their execution.
*   `publisher:component`: indicates the *status* from publication and notification subcomponent to **NetinDS**.
*   `uptime`: Indicates the runtime of the instance of **DriverAPI**.

In addition to the default subcomponents, there is a component with the name `${artifactId}:external`where `artifactId` is the name of our driver where the driver creator can add the *status* of elements specific to your implementation using the method [`updateSubcomponent`](#método-updateSubcomponent).

***

## Interface: **`SubcomponentDetail`**

The interface `SubcomponentDetail` indicates the relevant information about the *status* of the subcomponents that are part of the instance of the **DriverAPI**.

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `status` | `'pass' \| 'fail' \| 'warn'` | | Indicates the *status* of the subcomponent from the point of view of the *observability* of **NetinDS**. |
| `componentId` | `string` | | Driver *Universally Unique Identifier* (*UUID*). |
| `componentType` | `string` | | The type of subcomponent and metric indicated in this entry. |
| `links` | `object` | | `optional` Object with external links with information of interest for the current state of the subcomponent. |
| `metricUnit` | `string` | | `optional` Unit of measurement of the metric indicated in this entry. |
| `metricValue` | `any` | | `optional` The value of the metric indicated in this entry. |
| `output` | `string` | | `optional` Extra information about the current status of the **DriverAPI**. |
| `time` | `SubcomponentDetail` | | `optional` The date on which the status of this subcomponent was updated. |

***

## Interface: **`JobResult`**

The interface `JobResult` contains the relevant information about the execution of a publication or notification work.

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `createdAt` | `string` | | Timestamp in ISO format, with the date of creation of the work. |
| `resolvedAt` | `string` | | Timestamp in ISO format, with the date of resolution of the work. |
| `id` | `string` | | UUID of work. This unique identifier is returned by the publishing and notification functions at the time of applying for a new job. |
| `hasErrors` | `boolean` | `false` | A Boolean value that indicates whether the job failed. |
| `errors` | `{ datapoint: string; error: string \| string[] }[]` | `[]` | `optional` Array of objects. The property `datapoint` of these objects indicates the *Datapoint* Or *datapointSet* concrete where the error occurred while the property `error` indicates the type of error or errors logged. |
| `quantity` | `{ alarms: number; datapoints: number; timepoints: number }` | | Number of datapoints, alarms/events and timepoints that have been published in this paper. |
| `type` | `'map' \| 'table' \| 'alarm'` | | Type of work |

***

## Interface: **`DriverFailure`**

The interface `DriverFailure` contains the most relevant information for the registration of failures. Bugs can be added to the collection by using the [`addFailure`](#método-addfailure).

| Name | Type | Default | Description |
| :- | :- | :- |:- |
| `date` | `string` | | `optional` Timestamp of the record in ISO format, if none is indicated, the current time will be used. |
| `errors` | `{ error: string; source: string }[]` | | Array of errors. Each element is an object where the origin of the fault can be indicated by the property `source` and the corresponding fault through the property `error`. |
| `id` | `string` | | `optional` The identifier of the phallus, if none is indicated, a v4 UUID will be generated. |

***

## Enum: **`SubcomponentState`**

Defines the state of a subcomponent of **DriverAPI**.

| Member| Value | Description |
| :- | :- | :- |
| `running` | `0` | Subcomponent in operation and operating correctly. |
| `hold` | `1`| Subcomponent in operation, the operation is stopped or waiting for the release of resources in other subcomponents. |
| `stopped` | `2` | Subcomponent stopped, no type of operation is performed. |
| `error` | `3` | Subcomponent in operation and with failures in the operation. |

***

## Constructor: **`Driver`**

Creates a new instance of the Driver( class**DriverAPI**). Within the same process, only one instance can exist.

```typescript
Driver(options: DriverOptions): Driver
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `options` | [`DriverOptions`](#interfaz-driveroptions) | Driver configuration options. |

    **Returns:** `Driver`

***

## Property: [**`addresses`**](./Addresses.md)

Address repository, this object allows access to the collection that houses the addressing settings of the *Origins*, and their respective *datapointSets* and *datapoints*, which belong to this driver. Addressing defines the address of the "real" or "logical" data and information that the driver should publish to **NetinDS**.

***

## Property: [**`alarms`**](./Alarms.md)

Alarm repository, this object allows access to the collection that houses the events/alarms reported by the driver to **NetinDS**. This allows the driver to check the current status of the published alarms.

***

## Property: [**`notifier`**](./Notifier.md)

Notification interface, this object allows the driver to report alarms and events to **NetinDS**. you can check the status of events by using the interface that provides the object [`alarms`](#propiedad-alarmsalarmsmd).

***

## Property: [**`publisher`**](./Publisher.md)

Publishing interface, this object allows the driver to publish information to **NetinDS** through standardized structures.

***

## Property: **`configErrors`**

```typescript
configErrors: Multi | undefined
```

If there are errors in the driver configuration, both in the parameters indicated in the constructor and in the environment variables, these are indicated with an object of type [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b).

***

## Property: **`configState`**

```typescript
configState: SubcomponentState
```

Indicates the status of the **DriverAPI** using a variable of the type [`SubcomponentState`](#enum-subcomponentstate);

***

## Property: **`hasConfigErrors`**

```typescript
hasConfigErrors: boolean
```

A Boolean value that indicates whether there are any errors in the configuration, both in the parameters indicated in the constructor and in the environment variables.

***

## Property: **`id`**

```typescript
id: string
```

Driver *Universally Unique Identifier* (*UUID*)

***

## Property: **`isEnrolled`**

```typescript
isEnrolled: boolean
```

Boolean value that indicates whether the driver is subscribed to any instance of `NetinDS Agent`.

***

## Property: **`state`**

```typescript
state: SubcomponentState
```

Indicates the general status of the **DriverAPI** using a variable of the type [`SubcomponentState`](#interfaz-subcomponentstate).

***

## Property: **`status`**

```typescript
status: DriverHealthStatus
```

Indica of the *status* of the **DriverAPI** using an object of type [`DriverHealthStatus`](#interfaz-driverhealthstatus). This type of object is the one used in the interfaces of *observability* of **NetinDS**.

***

## Event: **`done`**

Issued when **DriverAPI** completes a publication or notification job.

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `jobId` | *String* | Job ID |
    | `result` | [`JobResult`](#interface-jobresult) | `optional` Final result of the work, if I finish without error |
    | `error` | [`Crash`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b)| `optional` Error, in case of work that ended with error |

***

## Event: **`update`**

Issued when a version of the driver configuration is available.

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `resourceId` | *String* | | configuration IDENTIFIER
    | `resourceDate` | [`JobResult`](#interface-jobresult) | Date of last update of the | configuration

***

## Event: **`updated`**

Issued when the driver configuration has been updated and is available.

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `resourceId` | *String* | | configuration IDENTIFIER
    | `resourceDate` | [`JobResult`](#interface-jobresult) | Date of last update of the | configuration

***

## Event: **`error`**

Issued when there is some kind of error in the **DriverAPI**.

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `error` | [`Crash`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b)| Error occurred in the **DriverAPI** |

## Event: **`state`**

Issued when any of the subcomponents of the **DriverAPI** changes its state.

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `subcomponentState` | [`SubcomponentState`](#enum-subcomponentstate)| Current status of subcomponent |

***

## Method: **`addFailure`**

Adds a new bug record to the bug collection. This collection belongs to the observability stack of **NetinDS** and I could be consulted about the url: `http://127.0.0.1:{PORT}/v1/registers`.

> `PORT` will have the value of the property `healthPort`, assigned in constructor **DriverAPI**. if this property has been initialized then the value will be that of the environment variable `CONFIG_SERVER_PORT`.

> the size of this list can be configured using the environment variable `CONFIG_CONFLICT_BUFFER_LENGTH`, whose default value is `100`.

```typescript
addFailure(failure: DriverFailure): void
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `failure` | [`DriverFailure`](#interfaz-driverfailure) | Object that defines the fault that wants to be added to the | collection

    **Returns:** `void`

***

## Method: **`update`**

Request the **DriverAPI** if you start a new configuration update job, this job will attempt to update all the resources in the instance of the **DriverAPI**, if the update successfully performs an event `updated` will be issued.

```typescript
update(): void
```

**Returns:** `void`

***

## Method: **`updateSubcomponent`**

Update or add the details of the status of a subcomponent, this should be used to inform about
the state of resources behind the driver API, for example states of connections with field
Devices.

> The maximum number of external subcomponents is 100.

```typescript
updateSubcomponent(subcomponent: SubcomponentDetail): boolean
```

*   **Parameters**

    | Name | Type | Description |
    | :------ | :------ | :------ |
    | `subcomponent` | [`SubcomponentDetail`](#interfaz-SubcomponentDetail) | Object of the subcomponent type, it will be added or updated if it already existed |

    **Returns:** `boolean`

## Method: **`disenroll`**

Starts the process of desuscribing the instance of the **DriverAPI** against the **NetinDS Agent**.

```typescript
disenroll(): Promise<void>
```

**Returns:** `Promise<void>`

***

## Method: **`enroll`**

Starts the process of subscribing to the instance of the **DriverAPI** against the **NetinDS Agent**.

```typescript
enroll(): Promise<void>
```

**Returns:** `Promise<void>`

***
