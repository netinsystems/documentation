# Addresses

The entity `addresses`inside ***NetinDS DriverAPI***, allows access to the address settings assigned to the driver. Each driver is identified by the artifact identifier (`artifactId`) indicated in the initialization of the instance of **DriverAPI**. During the process of *Enroll* of the **DriverAPI** in the system **NetinDS**, this will request all the *Origins* that contain datapoints that must be acquired by this driver. **NetinDS** make this association mediate the property `originType` of the `addressConfig` in the device template.

The templates in turn contain the property `originTypes`, type `array`, which stores the settings specified for that type of device for each driver. these configurations are made on an object of the type [`DriverConfig<T>`](#interfaz-driverconfigt) for each driver. [`DriverConfig<T>`](#interfaz-driverconfigt) has a single mandatory property, `originType`, which serves as the driver identifier (`artifactId`).

```yml
## Ejemplo de configuración para el driver `netin-ds-drv-snmp` y `netin-ds-drv-icmp`
originTypes:
  - originType: netin-ds-drv-snmp ## artifactId
    basicConfig:
      retries: 2
      address: '192.168.1.1'
      port: 161
      version: 2
      timeout: 5000
    snmpCommunities:
      readCommunity: public
      writeCommunity: private
    snmpV3Security:
      authAlgorithm: MD5
      privacyAlgorithm: DES
      usmUser: ''
      authPassword: ''
      privacyPassword: ''
  - originType: netin-ds-drv-icmp ## artifactId
    address: '192.168.1.1'
    count: 4
    timeout: 1000
```

Each driver in **NetinDS** has specific characteristics that depend on the implementation, the protocol used downstream of the **DriverAPI** and/or the types of devices or systems from which it should collect information. To be able to face these features, the config `addressConfig` allows you to define the necessary properties for numerous use cases, that is, the creator of a new driver can, through this *config* standard, determine the way in which the driver should behave when accessing information.

![NetinDS-Config](../../.assets/address-relationship.png)

It is therefore by the `addressConfig`, in the templates of **NetinDS**, and its counterpart interfaces [**`Datapoint`**](#interfaz-datapoint) and [**`DatapointItem`**](#interfaz-datapointitem), on the side of the **DriverAPI**, as the developer of a new driver can define how his driver should behave. All the properties of these interfaces are only interpreted by the driver in question.

For more information about the templates and their configuration at **NetinDS** see the [documentation](https://docs.netin.io/netin_webUI/NetinDS/templates).

## Table of Contents

*   [Addresses](#addresses)
    *   [Table of Contents](#tabla-de-contenidos)
    *   [Interface: **`Origins<T>`**](#interfaz-originst)
        *   [Example **`Origins<T>`**](#ejemplo-originst)
    *   [Interface: **`OriginsArray<T>`**](#interfaz-originsarrayt)
        *   [Example **`OriginsArray<T>`**](#ejemplo-originsarrayt)
    *   [Interface: **`Origin<T>`**](#interfaz-origint)
    *   [Interface: **`OriginItem<T>`**](#interfaz-originitemt)
    *   [Interface: **`DriverConfig<T>`**](#interfaz-driverconfigt)
    *   [Interface: **`DatapointSet`**](#interfaz-datapointset)
    *   [Interface: **`DatapointSetItem`**](#interfaz-datapointsetitem)
    *   [Interface: **`DatapointSetAddress`**](#interfaz-datapointsetaddress)
    *   [Interface: **`Datapoint`**](#interfaz-datapoint)
    *   [Interface: **`DatapointItem`**](#interfaz-datapointitem)
    *   [Method: **`getDatapoint`**](#método-getdatapoint)
    *   [Method: **`getDatapointList`**](#método-getdatapointlist)
    *   [Method: **`getDatapointSet`**](#método-getdatapointset)
    *   [Method: **`getOrigin`**](#método-getorigin)
    *   [Method: **`getOriginConfig`**](#método-getoriginconfig)
    *   [Method: **`getOrigins`**](#método-getorigins)
    *   [Method: **`getOriginsArray`**](#método-getoriginsarray)
    *   [Method: **`hasDatapoint`**](#método-hasdatapoint)
    *   [Method: **`hasDatapointSet`**](#método-hasdatapointset)
    *   [Method: **`hasOrigin`**](#método-hasorigin)

***

## Interface: **`Origins<T>`**

The interface `Origins<T>` defines the total of *Origins* for a driver in the form of an object (*Key*), each key being an identifier of *origin* and each value an object of the type [`Origin<T>`](#interfaz-origint).

| Name | Type | Default | Description |
| :----------------- | :------------------------------- | :------ | :---------------------------------------------------------------------------------- |
| `[origin: string]` | [`Origin<T>`](#interfaz-origint) |         | Object that defines addressing in the context of **NetinDS** of a *origin*. |

### Example **`Origins<T>`**

```json
{
  "10.10.50.1": {
    "driverConfig": {
      "driverId": "netin-ds-drv-snmp",
      "basicConfig": {
        "retries": 2,
        "address": "10.10.50.1",
        "port": 161,
        "version": 2,
        "timeout": 5000
      },
      "snmpCommunities": { "readCommunity": "public", "writeCommunity": "private" },
      "snmpV3Security": {
        "authAlgorithm": "MD5",
        "privacyAlgorithm": "DES",
        "usmUser": "",
        "authPassword": "",
        "privacyPassword": ""
      }
    },
    "deviceInfo": {
      "type": "map",
      "deviceDescription": {
        "type": "SIMPLE",
        "dataType": "STRING",
        "rawDataType": "OCTET_STRING",
        "address": "1.3.6.1.2.1.1.1.0",
        "accessMode": "read-only",
        "pollingGroup": "1d",
        "receiveMode": "polling"
      }
    }
  },
  "10.10.50.244": {
    "driverConfig": {
      "driverId": "netin-ds-drv-snmp",
      "basicConfig": {
        "retries": 2,
        "address": "10.10.50.244",
        "port": 161,
        "version": 2,
        "timeout": 5000
      },
      "snmpCommunities": { "readCommunity": "public", "writeCommunity": "private" },
      "snmpV3Security": {
        "authAlgorithm": "MD5",
        "privacyAlgorithm": "DES",
        "usmUser": "",
        "authPassword": "",
        "privacyPassword": ""
      }
    },
    "ifTable": {
      "type": "tableStatic",
      "address": { "rootAddress": "1.3.6.1.2.1.2.2", "indexes": ["ifIndex"] },
      "ifIndex": {
        "type": "SIMPLE",
        "dataType": "INTEGER",
        "rawDataType": "INTEGER",
        "address": "1.3.6.1.2.1.2.2.1.1",
        "accessMode": "read-only",
        "pollingGroup": "1h",
        "receiveMode": "polling"
      },
      "ifDescr": {
        "type": "SIMPLE",
        "dataType": "STRING",
        "rawDataType": "OCTET_STRING",
        "address": "1.3.6.1.2.1.2.2.1.2",
        "accessMode": "read-only",
        "pollingGroup": "1h",
        "receiveMode": "polling"
      },
      "ifType": {
        "type": "SIMPLE",
        "dataType": "INTEGER",
        "rawDataType": "INTEGER",
        "address": "1.3.6.1.2.1.2.2.1.3",
        "accessMode": "read-only",
        "pollingGroup": "1h",
        "receiveMode": "polling"
      }
    }
  }
}
```

***

## Interface: **`OriginsArray<T>`**

The interface `OriginsArray<T>` defines the total of *Origins* for a driver in the form of an array, each element of the array being an object of type [`OriginItem<T>`](#interfaz-originitemt).

```typescript
type OriginsArray<T> = OriginItem<T>[];
```

### Example **`OriginsArray<T>`**

```json
[
  {
    "origin": "10.10.50.1",
    "driverConfig": {
      "driverId": "netin-ds-drv-snmp",
      "basicConfig": {
        "retries": 2,
        "address": "10.10.50.1",
        "port": 161,
        "version": 2,
        "timeout": 5000
      },
      "snmpCommunities": {
        "readCommunity": "public",
        "writeCommunity": "private"
      },
      "snmpV3Security": {
        "authAlgorithm": "MD5",
        "privacyAlgorithm": "DES",
        "usmUser": "",
        "authPassword": "",
        "privacyPassword": ""
      }
    },
    "datapointSets": [
      {
        "datapointSetId": "deviceInfo",
        "type": "map",
        "datapoints": [
          {
            "datapointId": "deviceDescription",
            "type": "SIMPLE",
            "dataType": "STRING",
            "rawDataType": "OCTET_STRING",
            "address": "1.3.6.1.2.1.1.1.0",
            "accessMode": "read-only",
            "pollingGroup": "1d",
            "receiveMode": "polling"
          }
        ]
      }
    ]
  },
  {
    "origin": "10.10.50.244",
    "driverConfig": {
      "driverId": "netin-ds-drv-snmp",
      "basicConfig": {
        "retries": 2,
        "address": "10.10.50.244",
        "port": 161,
        "version": 2,
        "timeout": 5000
      },
      "snmpCommunities": {
        "readCommunity": "public",
        "writeCommunity": "private"
      },
      "snmpV3Security": {
        "authAlgorithm": "MD5",
        "privacyAlgorithm": "DES",
        "usmUser": "",
        "authPassword": "",
        "privacyPassword": ""
      }
    },
    "datapointSets": [
      {
        "datapointSetId": "ifTable",
        "type": "tableStatic",
        "address": {
          "rootAddress": "1.3.6.1.2.1.2.2",
          "indexes": ["ifIndex"]
        },
        "datapoints": [
          {
            "datapointId": "ifIndex",
            "type": "SIMPLE",
            "dataType": "INTEGER",
            "rawDataType": "INTEGER",
            "address": "1.3.6.1.2.1.2.2.1.1",
            "accessMode": "read-only",
            "pollingGroup": "1h",
            "receiveMode": "polling"
          },
          {
            "datapointId": "ifDescr",
            "type": "SIMPLE",
            "dataType": "STRING",
            "rawDataType": "OCTET_STRING",
            "address": "1.3.6.1.2.1.2.2.1.2",
            "accessMode": "read-only",
            "pollingGroup": "1h",
            "receiveMode": "polling"
          },
          {
            "datapointId": "ifType",
            "type": "SIMPLE",
            "dataType": "INTEGER",
            "rawDataType": "INTEGER",
            "address": "1.3.6.1.2.1.2.2.1.3",
            "accessMode": "read-only",
            "pollingGroup": "1h",
            "receiveMode": "polling"
          }
        ]
      }
    ]
  }
]
```

***

## Interface: **`Origin<T>`**

The interface `Origin<T>` defines addressing in the context of **NetinDS** of a *origin*.

| Name | Type | Default | Description |
| :------------------------- | :------------------------------------------- | :------ | :---------------------------------------------------------------------------------- |
| `driverConfig`             | [`DriverConfig<T>`](#interfaz-driverconfigt) |         | Object that defines the driver configuration for the *origin*.                     |
| `[datapointSetId: string]` | [`DatapointSet`](#interfaz-datapointset)     |         | Object that defines the addressing of a *datapointSet* in a *origin* concrete. |

***

## Interface: **`OriginItem<T>`**

The interface `OriginItem<T>` defines addressing in the context of **NetinDS** of a *origin*.

| Name | Type | Default | Description |
| :-------------- | :------------------------------------------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `origin`        | `string`                                           |         | Identifier of the *origin*.                                                                                                                                     |
| `driverConfig`  | [`DriverConfig<T>`](#interfaz-driverconfigt)       |         | Object that defines the driver configuration for the *origin*.                                                                                                 |
| `datapointSets` | [`DatapointSetItem[]`](#interfaz-datapointsetitem) |         | Array of objects of type [`DatapointSetItem`](#interfaz-datapointsetitem) that define the addressing of a *datapointSet* within a *origin* concrete. |

***

## Interface: **`DriverConfig<T>`**

The interface `DriverConfig<T>` is the union between `T`, interface defined by the driver creator, and the property `driverId`:

```typescript
export type DriverConfig<T> = T & { driverId: string };
```

Then, for a driver where the creator has defined the following interface:

```typescript
interface myDriverConfig {
  address: string;
  myConfigObject: {
    importantProperty: number;
    otherImportantProperty: string[];
  };
}
```

The combined interface would be:

```typescript
interface DriverConfig<myDriverConfig> {
  driverId: string;
  address: string;
  myConfigObject: {
    importantProperty: number;
    otherImportantProperty: string[];
  };
}
```

And a possible value of this:

```json
{
  "driverId": "myDriver",
  "address": "192.168.1.1",
  "myConfigObject": {
    "importantProperty": 7,
    "otherImportantProperty": ["one", "two"];
  }
};
```

***

## Interface: **`DatapointSet`**

The interface `DatapointSet` defines the addressing of a *datapointSet* in a *origin* concrete.

| Name | Type | Default | Description |
| :---------------------- | :----------------------------------------------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------- |
| `type`                  | `DatapointSetType`                                     |         | Indicates the type of datapointSet: `map` | `tableStatic` | `tableDynamic`                                                                |
| `address`               | [`DatapointSetAddress`](#interfaz-datapointsetaddress) |         | Object addressing at the level of *datapointSet* of a table type(`tableStatic` | `tableDynamic`).                                 |
| `[datapointId: string]` | [`Datapoint`](#interfaz-datapoint)                     |         | Object of the type [`Datapoint`](#interfaz-datapoint) that define the addressing of a datapoint within a *datapointSet* | concrete

***

## Interface: **`DatapointSetItem`**

The interface `DatapointSetItem` defines the addressing of a *datapointSet* in a *origin* concrete.

| Name | Type | Default | Description |
| :--------------- | :----------------------------------------------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| `datapointSetId` | `string`                                               |         | Identifier of the *datapointSet*                                                                                                                    |
| `type`           | `DatapointSetType`                                     |         | Indicates the type of *datapointSet*: `map` | `tableStatic` | `tableDynamic`                                                                          |
| `address`        | [`DatapointSetAddress`](#interfaz-datapointsetaddress) |         | `optional` Object that defines table addressing (`tableStatic` | `tableDynamic`).                                                       |
| `datapoints`     | [`DatapointItem[]`](#interfaz-datapointitem)           |         | Array of objects of type [`DatapointItem`](#interfaz-datapoint) that define the addressing of a datapoint within a particular datapointset |

***

## Interface: **`DatapointSetAddress`**

The interface `DatapointSetAddress` defines the addressing of tables in datapoints of type table (`tableStatic` | `tableDynamic`). Allows you to define a global address for tables and the indexes of these tables.

This feature of the *datapointSet* table type allows the creator of a new driver to add an extra level of addressing, with the same flexibility as the field `address` of the interfaces [**`Datapoint`**](#interfaz-datapoint) and [**`DatapointItem`**](#interfaz-datapointitem), for specific use cases.

Some examples can be:

*   This address is used in the driver `netin-ds-drv-snmp`to define the address of an SNMP table.
*   It is used by SQL driver implementations to define the table on which the query is made and using the field `address`of the datapoint as the column indicator.
*   ...

For more information see the [documentation](https://docs.netin.io/netin_webUI/NetinDS/templates).

| Name | Type | Default | Description |
| :------------ | :-------- | :------ | :------------------------------------------ |
| `rootAddress` | `string`  |         | Table-level address of the datapointSet |
| `indexes`     | `sting[]` |         | List of indexes in table |

***

## Interface: **`Datapoint`**

The interface `Datapoint` defines the addressing of a datapoint.

| Name | Type | Default | Description |
| :------------- | :-------------- | :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `type`         | `DatapointType` |         | Datapoint type, indicates how the datapoint will be treated within the system **NetinDS**. Review the documentation on [templates](https://docs.netin.io/netin_webUI/NetinDS/templates) from Netin for more information. |
| `dataType`     | `DataType`      |         | Field type `value`/`rawValue` expected in the publication of this datapoint. This field is informative and its use is dependent on the implementation of the driver.                                                                   |
| `rawDataType`  | `string`        |         | The type of the data at its source. This field is informative and its use is dependent on the implementation of the driver. <br> ***Format***: `/^[a-zA-Z0-9-_]{1,80}$/`                                                                          |
| `address`      | `string`        |         | Addressing the data at its source. This field is informative and its use is dependent on the implementation of the driver. <br> ***Format***: `/^[\w\d_&#:$.-]{1,80}$/`                                                              |
| `accessMode`   | `AccessType`    |         | Level of access to the data at its source. This field is informative and its use is dependent on the implementation of the driver.                                                                                                             |
| `receiveMode`  | `ReceiveMode`   |         | Data reception mode. This field is informative and its use is dependent on the implementation of the driver.                                                                                                                        |
| `pollingGroup` | `PollingGroup`  |         | Polling group, for `receiveMode: "polling"`. This field is informative and its use is dependent on the implementation of the driver.                                                                                                  |

***

## Interface: **`DatapointItem`**

The interface `DatapointIem` defines the addressing of a datapoint.

| Name | Type | Default | Description |
| :------------- | :-------------- | :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `datapointId`  | `string`        |         | | datapoint ID
| `type`         | `DatapointType` |         | Datapoint type, indicates how the datapoint will be treated within the system **NetinDS**. Review the documentation on [templates](https://docs.netin.io/netin_webUI/NetinDS/templates) from Netin for more information. |
| `dataType`     | `DataType`      |         | Field type `value`/`rawValue` expected in the publication of this datapoint. This field is informative and its use is dependent on the implementation of the driver.                                                                   |
| `rawDataType`  | `string`        |         | The type of the data at its source. This field is informative and its use is dependent on the implementation of the driver. <br> ***Format***: `/^[a-zA-Z0-9-_]{1,80}$/`                                                                          |
| `address`      | `string`        |         | Addressing the data at its source. This field is informative and its use is dependent on the implementation of the driver. <br> ***Format***: `/^[\w\d_&#:$.-]{1,80}$/`                                                              |
| `accessMode`   | `AccessType`    |         | Level of access to the data at its source. This field is informative and its use is dependent on the implementation of the driver.                                                                                                             |
| `receiveMode`  | `ReceiveMode`   |         | Data reception mode. This field is informative and its use is dependent on the implementation of the driver.                                                                                                                        |
| `pollingGroup` | `PollingGroup`  |         | Polling group, for `receiveMode: "polling"`. This field is informative and its use is dependent on the implementation of the driver.                                                                                                  |

***

## Method: **`getDatapoint`**

Returns the configuration of the requested datapoint.

```typescript
getDatapoint(origin: string, datapointSetId: string, datapointId: string): Datapoint | undefined
```

*   **Parameters**

    | Name | Type | Description |
    | :--------------- | :------- | :---------------------- |
    | `origin`         | *String* | Requested source |
    | `datapointSetId` | *String* | DatapointSet requested |
    | `datapointId`    | *String* | Datapoint requested |

    **Returns:** [`Datapoint`](#interfaz-datapoint) | `undefined`

***

## Method: **`getDatapointList`**

```typescript
getDatapointList(origin: string, datapointSet: string): string[]
```

Returns the list of datapoints that make up a particular datapointset in a specific source.

*   **Parameters**

    | Name | Type | Description |
    | :------------- | :------- | :---------------------- |
    | `origin`       | *String* | Requested source |
    | `datapointSet` | *String* | DatapointSet requested |

    **Returns:** `string[]`

***

## Method: **`getDatapointSet`**

Returns the configuration of a specific datapointSet to a specific source

```typescript
getDatapointSet(origin: string, datapointSetId: string): DatapointSet | undefined
```

*   **Parameters**

    | Name | Type | Description |
    | :--------------- | :------- | :---------------------- |
    | `origin`         | *String* | Requested source |
    | `datapointSetId` | *String* | DatapointSet requested |

    **Returns:** [`DatapointSet`](#interfaz-datapointset) | `undefined`

***

## Method: **`getOrigin`**

Returns the configuration of a specific source.

```typescript
getOrigin<T>(origin: string): Origin<T> | undefined
```

*   **Parameters**

    | Name | Type | Description |
    | :------- | :------- | :---------------- |
    | `origin` | *String* | Requested source |

    **Returns:** [`Origin<T>`](#interfaz-origint) | `undefined`

***

## Method: **`getOriginConfig`**

Returns the driver settings for a specific source.

```typescript
getOriginConfig<T>(origin: string): DriverConfig<T> | undefined
```

*   **Parameters**

    | Name | Type | Description |
    | :------- | :------- | :---------------- |
    | `origin` | *String* | Requested source |

    **Returns:** [`DriverConfig<T>`](#interfaz-driverconfigt) | `undefined`

***

## Method: **`getOrigins`**

Returns the configuration of all system sources for the driver in object format (JSON).

```typescript
getOrigins<T>(): Origins<T>
```

**Returns:** [`Origins<T>`](#interfaz-originst)

***

## Method: **`getOriginsArray`**

Returns the configuration of all system sources for the driver in array format.

```typescript
getOriginsArray<T>(): OriginsArray<T>
```

**Returns:** [`OriginsArray<T>`](#interfaz-originsArrayt)

***

## Method: **`hasDatapoint`**

Checks whether a particular datapoint is present in the configuration.

```typescript
hasDatapoint(origin: string, datapointSetId: string, datapointId: string): boolean
```

*   **Parameters**

    | Name | Type | Description |
    | :--------------- | :------- | :---------------------- |
    | `origin`         | *String* | Requested source |
    | `datapointSetId` | *String* | DatapointSet requested |
    | `datapointId`    | *String* | Datapoint requested |

    **Returns:** `boolean`

***

## Method: **`hasDatapointSet`**

Checks whether a particular datapointSet is present in the configuration.

```typescript
hasDatapointSet(origin: string, datapointSet: string): boolean
```

*   **Parameters**

    | Name | Type | Description |
    | :------------- | :------- | :---------------------- |
    | `origin`       | *String* | Requested source |
    | `datapointSet` | *String* | DatapointSet requested |

    **Returns:** `boolean`

***

## Method: **`hasOrigin`**

Checks whether a specific source is present in the configuration.

```typescript
hasOrigin(origin: string): boolean
```

*   **Parameters**

    | Name | Type | Description |
    | :------- | :------- | :---------------- |
    | `origin` | *String* | Requested source |

    **Returns:** `boolean`
