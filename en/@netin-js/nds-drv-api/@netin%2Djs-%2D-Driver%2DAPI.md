# @netin-js/nds-driver-api

***

## Table of Contents

*   [Introduction](#introducción)
*   [Installation](#instalación)
*   [Information](#información)
*   [Use](#uso)

## Introduction

The objective of **NetinDS DriverAPI** is to facilitate the development of "drivers" that allow the integration of new sources of information. Within the ecosystem **Netin Systems**, the drivers are responsible for the capture and publication of information (*datapoints* and *timepoints*) and events (*alarms*) from the field elements ([shop floor](https://en.wikipedia.org/wiki/Shop_floor)) and third-party systems to the kernel of the **NetinDS**.

![NetinDS-Architecture](../.assets/netinds-architecture.png)

**NetinDS DriverAPI** Allows:

*   Run datapoint structure publishing jobs in map and table format.
*   Run time series publishing jobs (timepoints).
*   Run event and alarm notification jobs.

## Installation

**NetinDS DriverAPI** any package manager compatible with Node.js® is installed.

*   **npm**

```sh
npm install @netin-js/nds-drv-api
```

*   **yarn**

```sh
yarn add @netin-js/nds-drv-api
```

## Information

The following steps will guide you through the process of creating a device template that uses your new driver and the configuration itself for publishing events and information to a system. **NetinDS**.

> **To complete the process it will be necessary that you have a NetinDS system with at least one working agent**

### **Step 1: Generate a device template**

The following is the template (in YAML format) that will serve as the basis for the example developed in these steps. This template declares some datapoints to be published by our driver which we will identify with the `originType: 'custom-dvr'`.

```yaml
templateId: 'CustomTemplate'
templateVersion: '0.0.1'
alias: 'Custom driver template'
description: 'Custom devices with very interesting information'
origin: '/{ICMP_IPAddress}/'
# Configuración de los drivers que intervienen en la recolección de datos de este dispositivo
originTypes:
  # la configuración de nuestro driver para cada origin
  - originType: 'custom-dvr' # Nuestro identificador de driver
    address: '/{ICMP_IPAddress}/'
    customProperty: 3
    otherCustomProperty: 'importantConfig'
# Configuración de los datapointSets
datapointSets:
  - datapointSetType: map
    datapointSetId: myMap
    alias: My map datapointSet
    description: This is an important map
    # Configuración de datapoints
    datapoints:
      - commonConfig:
          datapointId: firstDatapoint
          alias: First datapoint
          description: My first datapoint in map
          syntaxInfo: This datapoint is a boolean
          datapointType: DATAPOINT
        addressConfig:
          dataType: BOOLEAN
          originType: 'custom-drv' # Nuestro identificador de driver
          originDataType: 'boolean'
          originAddress: 'firstData' # Nuestro propio direccionamiento
          originAccessType: 'readOnly'
          receiveMode: 'polling'
          pollingGroup: '1m'
      - commonConfig:
          datapointId: secondDatapoint
          alias: Second datapoint
          description: My second datapoint in map
          syntaxInfo: This datapoint is an integer
          datapointType: '#TIMEPOINT'
        addressConfig:
          dataType: INTEGER
          originType: 'custom-drv' # Nuestro identificador de driver
          originDataType: 'number'
          originAddress: 'secondData' # Nuestro propio direccionamiento
          originAccessType: 'readOnly'
          receiveMode: 'polling'
          pollingGroup: '1m'
  - datapointSetType: tableDynamic
    datapointSetId: myTable
    alias: My table datapointSet
    description: This is an important table
    tableAddress:
      rootTable: firstArray
      indexes: 
        - secondDatapoint
    # Configuración de datapoints
    datapoints:
      - commonConfig:
          datapointId: firstDatapoint
          alias: First datapoint
          description: My first datapoint in table
          syntaxInfo: This datapoint is a boolean
          datapointType: DATAPOINT
        addressConfig:
          dataType: BOOLEAN
          originType: 'custom-drv' # Nuestro identificador de driver
          originDataType: 'boolean'
          originAddress: 'firstData' # Nuestro propio direccionamiento
          originAccessType: 'readOnly'
          receiveMode: 'polling'
          pollingGroup: '1m'
      - commonConfig:
          datapointId: secondDatapoint
          alias: Second datapoint
          description: My second datapoint in table
          syntaxInfo: This datapoint is an integer
          datapointType: '#TIMEPOINT'
        addressConfig:
          dataType: INTEGER
          originType: 'custom-drv' # Nuestro identificador de driver
          originDataType: 'number'
          originAddress: 'secondData' # Nuestro propio direccionamiento
          originAccessType: 'readOnly'
          receiveMode: 'polling'
          pollingGroup: '1m'
```

For more information on how to create a template check the [documentation](https://docs.netin.io/).

### **Step 2: Load the device template on a NetinDS (Dragonfly) system**

To complete this step follow the instructions in the [documentation](https://docs.netin.io/netin_webUI/NetinDS/standalone/settings-menu/standalone-repository/#plantillas) of **NetinDS**.

### **Step 3: Driver enrollment process**

With the template loaded on our system **NetinDS** we can start programming our new driver. In general, it is recommended to follow the following steps:

*   Create an instance of our driver `'custom-drv'`.
*   Subscribe to the events of our driver instance in order to be able to handle the flow of updates, the appearance of errors and state changes.
*   Request the subscription process with the **NetinDS Agent**.

```typescript
// Importamos el API en nuestro código
import DriverAPI, {Multi, Crash} from '@netin-js/nds-drv-api';

// Creamos la instancia de nuestro driver
const driver = new DriverAPI({ artifactId: 'custom-drv' });

// *************************************************************************************************
// #region Gestión de eventos del driver
driver.on('error', error => {
  console.log(`Error in driver: ${error.message}`);
  if (error instanceof Multi || error instanceof Crash) {
    error.trace().forEach(entry => console.log(entry));
  }
});
driver.on('update', () => {
  console.log(`New configuration for the driver`);
  driver.update();
});
driver.on('updated', (resourceId: string, resourceDate: string) => {
  console.log(`Configuration updated ${resourceId} in ${resourceDate}`);
});
driver.on('done', (jobId: string, result?: JobResult, error?: Crash) => {
  if (error) {
    console.log(`Job [${jobId}] was finished with errors: ${error.message}`);
  } else if (result) {
    console.log(`Job [${jobId}] finished successfully: ${JSON.stringify(result, null, 2)}`);
  }
});
driver.on('state', (state: Types.State.DriverState) => {
  console.log(`The driver state change to ${state}`);
});
// #endregion
// *************************************************************************************************
// #region Suscripción del driver al agente
driver
   .enroll()
   .then(() => {
      console.log('Enroll performed');
   })
   .catch((error: Crash | Multi | Error) => {
      console.log(`Error performing enroll: ${error.message}`);
      if (error instanceof Crash || error instanceof Multi) {
          console.log(`${error.trace()}`);
        }
   });
// #endregion
```

### **Step 4: Management of addressing our driver**

Once our instance is subscribed, and after receiving the event `updated`, we will have access to the configuration of our driver. Below is an example of how to perform this management based on the following steps:

*   Declaration of the specific configuration interface of our driver `custom-drv`.
*   Access to the configuration of all origins in array format.
*   Access to the configuration of all origins in object format.

```typescript
...

interface MyCustomDrvConfig {
  address: string;
  customProperty: number;
  otherCustomProperty: string;
}
driver.on('update', () => {
  console.log(`New configuration for the driver`);
  driver.update();
});
driver.on('updated', (resourceId: string, resourceDate: string) => {
  console.log(`Configuration updated ${resourceId} in ${resourceDate}`);
  console.log(`Origins in Object format`);
  console.log(driver.addresses.getOrigins<MyCustomDrvConfig>());
  console.log(`Origins in Array format`);
  console.log(driver.addresses.getOriginsArray<MyCustomDrvConfig>());
});
...

```

The execution of the code shown, on a system where there was only one computer on which we want to perform data collection, and whose `ICMP_IPAddress` outside `192.168.1.1`, will have the following output

```console
New configuration for the driver
Configuration updated 33a64df551425fcc55e4d42a148795d9f25f89d4 in 2021-05-16T14:00:09Z
Origins in Object format
{
  "192.168.1.1": {
    "driverConfig": {
      "driverId": "custom-drv",
      "address": "192.168.1.1",
      "customProperty": 3,
      "otherCustomProperty": "importantConfig"
    },
    "myMap": {
      "type": "map",
      "firstDatapoint": {
         "type": "DATAPOINT",
         "dataType": "BOOLEAN",
         "rawDataType": "boolean",
         "address": "firstData",
         "accessMode": "read-only",
         "pollingGroup": "1m",
         "receiveMode": "polling"
      },
      "secondDatapoint": {
         "type": "#TIMEPOINT",
         "dataType": "INTEGER",
         "rawDataType": "number",
         "address": "secondData",
         "accessMode": "read-only",
         "pollingGroup": "1m",
         "receiveMode": "polling"
      }
    },
    "myTable": {
      "type": "tableDynamic",
      "address": {
        "rootAddress": "firstArray",
        "indexes": ["secondDatapoint"]
      },
      "firstDatapoint": {
         "type": "DATAPOINT",
         "dataType": "BOOLEAN",
         "rawDataType": "boolean",
         "address": "firstData",
         "accessMode": "read-only",
         "pollingGroup": "1m",
         "receiveMode": "polling"
      },
      "secondDatapoint": {
         "type": "#TIMEPOINT",
         "dataType": "INTEGER",
         "rawDataType": "number",
         "address": "secondData",
         "accessMode": "read-only",
         "pollingGroup": "1m",
         "receiveMode": "polling"
      }
    }
  }
}
Origins in Array format
[
  {
    "origin": "192.168.1.1",
    "driverConfig": {
      "driverId": "custom-drv",
      "address": "192.168.1.1",
      "customProperty": 3,
      "otherCustomProperty": "importantConfig"
    },
    "datapointSets": [
      {
        "datapointSetId": "myMap",
        "type": "map",
        "datapoints": [
          {
            "datapointId": "firstDatapoint",
            "type": "DATAPOINT",
            "dataType": "BOOLEAN",
            "rawDataType": "boolean",
            "address": "firstData",
            "accessMode": "read-only",
            "pollingGroup": "1m",
            "receiveMode": "polling"            
          },
          {
            "datapointId": "secondDatapoint",
            "type": "#TIMEPOINT",
            "dataType": "INTEGER",
            "rawDataType": "number",
            "address": "secondData",
            "accessMode": "read-only",
            "pollingGroup": "1m",
            "receiveMode": "polling"           
          }
        ]
      },
      {
        "datapointSetId": "myTable",
        "type": "tableDynamic",
        "address": {
          "rootAddress": "firstArray",
          "indexes": ["secondDatapoint"]
        },
        "datapoints": [
          {
            "datapointId": "firstDatapoint",
            "type": "DATAPOINT",
            "dataType": "BOOLEAN",
            "rawDataType": "boolean",
            "address": "firstData",
            "accessMode": "read-only",
            "pollingGroup": "1m",
            "receiveMode": "polling"            
          },
          {
            "datapointId": "secondDatapoint",
            "type": "#TIMEPOINT",
            "dataType": "INTEGER",
            "rawDataType": "number",
            "address": "secondData",
            "accessMode": "read-only",
            "pollingGroup": "1m",
            "receiveMode": "polling"           
          }
        ]
      }      
    ]    
  }
]
```

### **Step 5: Publish and notify**

With our driver subscribed to **NetinDS Agent** and now that we know the configuration of *Origins*, *datapointSets* and *datapoints* that we are expected to read may move on to post information and report events.

```typescript
// Ejemplo de conjunto de datos
const myArray = [
  { firstData: true, secondData: 4 },
  { firstData: true, secondData: 7 },
  { firstData: false, secondData: 10 }
]
const myMap = {
  firstData: true,
  secondData: 3,
}

//Publicación de mapas
driver.publisher.publish('192.168.1.1', 'myMap', myMap);

//Publicación de tablas
driver.publisher.publishBulk({
  origin: '192.168.1.1',
  datapointSetId: 'myTable',
  data: myArray.map(entry =>
    Object.entries(entry).map(([datapointId, rawValue]) => ({
      datapointId,
      rawValue,
    }))
  ),
});

// Notificación de alarmas
driver.notifier.error({
  origin: '192.168.1.1',
  datapointSetId: 'myMap',
  datapointId: 'firstDatapoint',
  text: 'My alarm text',
  textHelp: 'My help text',
});

// Notificación de alarmas de multi-instancia
driver.notifier.warn({
  origin: '192.168.1.1',
  datapointSetId: 'myMap',
  datapointId: 'firstDatapoint',
  text: 'My alarm text',
  textHelp: 'My help text',
  codes: ['thisMakeMySpecial']
});

// Desactivación de las alarmas notificadas
driver.notifier.deactivate({
  origin: '192.168.1.1',
  datapointSetId: 'myMap',
  datapointId: 'firstDatapoint',
  text: 'My alarm text',
  textHelp: 'My help text',
  codes: ['thisMakeMySpecial']
});
driver.notifier.deactivate({
  origin: '192.168.1.1',
  datapointSetId: 'myMap',
  datapointId: 'firstDatapoint',
  text: 'My alarm text',
  textHelp: 'My help text',
});
```

> Note that no differences are necessary *DATAPOINTS* of *TIMEPOINTS*, this is something that **DriverAPI** will do for us according to the configuration of the template.

## Use

### Driver with peaks of information and occupation of the *data path*

If your driver generates a lot of information you may have to face some problems:

*   Publication of *DATAPOINTS* in the *data path*: the *data path* of **NetinDS** works as a *tail*the *Kernel* of the **NetinDS Agent** is responsible for consuming the data as they reach the publication queues, to prevent a driver from overloading the system there is an intrinsic protection of the system: if the data to be published still exists in the queue, it will not be published and the work will end with error.
*   Creation of a large number of jobs: by default the maximum value of parallel jobs that can be created is `100`, if you need to increase this number of jobs to be able to face information spikes where you need to publish a large number of jobs, you can do this by using the environment variable `CONFIG_STREAM_HIGH_WATER_MARK`. Remember that increasing this value will increase the resource consumption of the node.

### Add information from our driver to netinDS observability

**DriverAPI** allows you to publish information relevant to the internal state of our driver in the observability interface of **NetinDS**, for this we can use the methods [`addFailure`](./docs/Driver.md#método-addfailure) and [`updateSubcomponent`](./docs/Driver.md#método-updatesubcomponent).

```typescript
// Publicación de fallos
driver.addFailure({
  errors: [{ error: `My error text`, source: `The Source` }],
});

//Publicación de subcomponentes
driver.updateSubcomponent({ status: 'warn', componentId: 'myComponent' });
```

You can purchase the status of your driver, including publishing your own bugs and components on endpoints:

*   `http://127.0.0.1:{PORT}/v1/health`
*   `http://127.0.0.1:{PORT}/v1/registers`

> `PORT` will have the value of the property `healthPort`, assigned in constructor **DriverAPI**. if this property has been initialized then the value will be that of the environment variable `CONFIG_SERVER_PORT`.

### Retries of the subscription process

In order to ensure that our driver performs the subscription process properly at the start of the system it is common to have to implement some algorithm that allows you to retry this operation indefinitely until you get it.

The following is a code example with the implementation of retries with interval increase to a certain maximum using two auxiliary functions.

```typescript
const MAX_WAIT_TIME = 15000;
const WAIT_TIME = 1000;

/**
 * Auxiliar function to perform the wait process
 * @param delay - time in milliseconds to wait for retry
 * @returns
 */
function wait(delay: number): Promise<void> {
  return new Promise(resolve => {
    console.log(`Enroll will be retry in ${delay} milliseconds`);
    setTimeout(resolve, delay);
  });
}
/**
 * Perform the retry functionality
 * @param waitTime - Tme to wait in case of failure
 * @param maxWaitTime - Max time to wait
 * @param attempt - Retry counter
 * @returns
 */
function retryEnroll(waitTime: number, maxWaitTime: number, attempt = 1): Promise<void> {
  return new Promise((resolve, reject) => {
    driver
      .enroll()
      .then(() => {
        console.log('Enroll performed');
      })
      .catch((error: Crash | Multi | Error) => {
        console.log(`Error performing enroll: ${error.message}`);
        if (error instanceof Crash || error instanceof Multi) {
          console.log(`${error.trace()}`);
        }
        wait(waitTime).then(() => {
          const newWaitTime =
            waitTime + attempt * WAIT_TIME > maxWaitTime
              ? maxWaitTime
              : waitTime + attempt * WAIT_TIME;
          retryOperation(newWaitTime, maxWaitTime, attempt + 1)
            .then(resolve)
            .catch(reject);
        });
      });
  });
}
retryEnroll(WAIT_TIME, MAX_WAIT_TIME);
```

### API

*   [Driver](./@netin%2Djs-%2D-Driver%2DAPI/Driver.md)
*   [Publisher](./@netin%2Djs-%2D-Driver%2DAPI/Publisher.md)
*   [Notifier](./@netin%2Djs-%2D-Driver%2DAPI/Notifier.md)
*   [Addresses](./@netin%2Djs-%2D-Driver%2DAPI/Addresses.md)
*   [Alarms](./@netin%2Djs-%2D-Driver%2DAPI/Alarms.md)

### Environment variables

#### NetinDS-DriverAPI General Configuration

*   **`CONFIG_ENTITY_ID`** default(`uninitialized`): UNIVOC identifier in UUID format (RFC4122) of the **NetinDS Agent** to which this instance of **DriverAPI**. Initially it is not necessary to configure this value since it is usually indicated by the discovery service:
    *   In legacy mode, that is, in legacy mode **NetinDS Dragonfly** this identifier is selected from the configuration database.
    *   In **NetinDS Spider** this identifier is indicated by the discovery service.
*   **`CONFIG_ARTIFACT_RELEASE`** default(`0.0.0`): defines the release and version to assign to the **DriverAPI** in the context of the observability of **NetinDS**.
*   **`CONFIG_ENABLED_PERIOD_COT`** default(`false`): Enables alarm sending mode with `CoT`(*Cause of Transmission*) `Period`, that is, that the server be notified of the status of an alarm in each evaluation event.
    > ***Its use is not recommended, if it is not for special cases where a constant check of the status of the alarms is needed.***
*   **`CONFIG_STREAM_HIGH_WATER_MARK`** default(`100`): indicates the size of the internal buffers of **DriverAPI** used for backpressure functions. **DriverAPI** It has several processing buffers, so the total size of the buffer depends on the sum of all of them, and the type of data to be processed.
    > ***The objective of this buffer is to adjust stably to the data peaks, without making an excessive consumption of resources, this buffer on alternative to the buffers of the Firehorse should not be used.***
*   **`CONFIG_SERVER_PORT`** default(`3000`): port on which the observability REST API is offered **NetinDS**this value is applied only if the property has not been initialized `healthPort` in the constructor of the **DriverAPI**. This REST API will be offered on the endpoints:
    *   `http://127.0.0.1:{PORT}/v1/alarms`: log of alarms issued from **DriverAPI** to the server of **NetinDS** and its status.
    *   `http://127.0.0.1:{PORT}/v1/halted-origins`: record of sources that have been filtered at boot of **DriverAPI** for containing errors.
    *   `http://127.0.0.1:{PORT}/v1/health`: status of the internal subcomponents of **DriverAPI** .
    *   `http://127.0.0.1:{PORT}/v1/metrics`: metrics of the **DriverAPI** in format [Prometheus](https://prometheus.io/).
    *   `http://127.0.0.1:{PORT}/v1/quarantine`— Logging datapointsets or datapoints in a quarantined state, that is, datapoints whose configuration is incorrect.
    *   `http://127.0.0.1:{PORT}/v1/registers`: log of the last publish jobs to the server that ended with some type of error. The size of this list can be configured using the environment variable: `CONFIG_CONFLICT_BUFFER_LENGTH`, whose default value is `100`.
        > ***Modifying the variable is not recommended `CONFIG_CONFLICT_BUFFER_LENGTH` if you don't know what you're doing.***
*   **`CONFIG_SERVER_HOST`** default(`localhost`): IP address on which the observability REST API is associated.
*   **`CONFIG_SERVER_MODE`** default(`http`): How the observability REST API works. Valid options are `http` or `https`, the latter forces the configuration of certificates for the server.
*   **`CONFIG_SERVER_FILE_CERT_PATH`** default(`certs/cert.pem`): Path to the certificate file for the server. Only used in mode `https`.
*   **`CONFIG_SERVER_FILE_KEY_PATH`** default(`certs/key.pem`): Path to the private key file. Only used in mode `https`.

#### Configuring the Discovery Service

The discovery service is responsible for the management of the devices/systems that are monitored from a **NetinDS**.

*   **`CONFIG_DISCOVERY_HTTP_ADDR`** default(`http://127.0.0.1:29800`): Discovery service URL.
    > ***If this variable is not defined, the system will work in legacy mode, that is, in NetinDS DragonFly mode, requesting the configuration directly to the database.***
*   **`CONFIG_DISCOVERY_ADDRESSES_HTTP_PATH`** default(`/api/v1/addresses`): Path of the DISCOVERY SERVICE API to obtain 'addresses' belonging to devices/systems.
*   **`CONFIG_DISCOVERY_RESOURCES_HTTP_PATH`** default(`/api/v1/resources`): Discovery service API path for obtaining resources (global variables, configuration files ...).
*   **`CONFIG_DISCOVERY_ENROLL_HTTP_PATH`** default(`/api/v1/enroll`): Discovery service API path for requesting enrollment of new devices or systems.
*   **`CONFIG_DISCOVERY_UPDATE_INTERVAL`** default(`60000`): Indicates time interval for checking for updates in the system configuration.
*   **`CONFIG_DISCOVERY_OPEN_TIMEOUT`** default(`2000`): Maximum waiting time for connection establishment.
*   **`CONFIG_DISCOVERY_RESPONSE_TIMEOUT`** default(`5000`): Maximum timeout for discovery service response in the connection process. This time begins to count from the time the connection is open.
*   **`CONFIG_DISCOVERY_READ_TIMEOUT`** default(`5000`): Maximum timeout for receiving the response to configuration read requests.
*   **`CONFIG_DISCOVERY_AUTO_UPDATE`** default(`false`): Instructs the system to work with automatic updates, that is, not to wait for confirmation that it should update.
    > ***It is not recommended to modify the default value if you do not know exactly what is being done.***
*   **`CONFIG_DISCOVERY_LAZY_START`** default(`false`): Instructs the system to make the connection to the Discovery service at boot.
    > ***It is not recommended to modify the default value if you do not know exactly what is being done.***
*   **`CONFIG_DISCOVERY_HTTP_TOKEN`** default(`false`): Access token to the Discovery Service API, which is required when access control is enabled.
*   **`CONFIG_DISCOVERY_HTTP_SSL`** default(`false`): Enables the use of the communications scheme under HTTPS.
*   **`CONFIG_DISCOVERY_HTTP_SSL_VERIFY`** default(`true`): Enables verification of discovery server certificates if `CONFIG_DISCOVERY_HTTP_SSL_VERIFY`is enabled.
*   **`CONFIG_DISCOVERY_CA_PATH`** default(`undefined`): Path to the DISCOVERY SERVICE CA certificate file. Only used if `CONFIG_DISCOVERY_HTTP_SSL` is active.
*   **`CONFIG_DISCOVERY_CLIENT_CERT_PATH`** default(`undefined`): Path to the client certificate file required for access to the discovery service. Only used if `CONFIG_DISCOVERY_HTTP_SSL` is active.
*   **`CONFIG_DISCOVERY_CLIENT_KEY_PATH`** default(`undefined`): Path to the client certificate key file required for access to the discovery service. Only used if `CONFIG_DISCOVERY_HTTP_SSL` is active.
*   **`CONFIG_DISCOVERY_TLS_SERVER_NAME`** default(`undefined`): server name to be used as [SNI](https://es.wikipedia.org/wiki/Server_Name_Indication) when the connection occurs in TLS. Only used if `CONFIG_DISCOVERY_HTTP_SSL` is active.

#### Configuring Access to the Configuration Database (Legacy)

*   **`CONFIG_MONGO_ADDR`** default(`mongodb://127.0.0.1:28900/netin`): URL where the agent configuration database is located **NetinDS**.
*   **`CONFIG_MONGO_SERVER_SELECTION_TIMEOUT`** default(`5000`— Maximum timeout in the process of selecting a server to perform an operation. It is only used if the configuration database is working in cluster mode.
*   **`CONFIG_MONGO_POOL_SIZE`** default(`4`): Maximum size of the stack of connections against the database.
*   **`CONFIG_MONGO_KEEP_ALIVE`** default(`true`—Enables keepalive on connections against the database, thus remaining set.
*   **`CONFIG_MONGO_KEEP_ALIVE_INITIAL_DELAY`** default(`10000`): Timeout before the initial keepalive on connections against the database.
*   **`CONFIG_MONGO_CONNECTION_TIMEOUT`** default(`10000`— Maximum timeout for an attempt to connect to the database.
*   **`CONFIG_MONGO_SOCKET_TIMEOUT`** default(`10000`): Maximum timeout for send/receive requests over the socket used to connect to the database.

#### Configuring access to the real-time database (Redis)

*   **`CONFIG_REDIS_ADDR`** default(`redis://127.0.0.1:28910/0`): URL where the real-time database of the agent is located **NetinDS**.
*   **`CONFIG_REDIS_PASSWORD`** default(`undefined`): Real-time database access password.
*   **`CONFIG_REDIS_KEEPALIVE`** default(`10000`): The time interval between keepalive in the connection sockets to the real-time database.
*   **`CONFIG_REDIS_LAZY_START`** default(`true`): Instructs the system to make the connection to the real-time database at boot.
    > ***It is not recommended to modify the default value if you do not know exactly what is being done.***
*   **`CONFIG_REDIS_CONNECTION_TIMEOUT`** default(`10000`— Maximum timeout for an attempt to connect to the database.
*   **`CONFIG_REDIS_RETRY_DELAY_FACTOR`** default(`2000`): Time between retries connecting to the real-time database. The time will be gradually increased in each after each failure based on the value of this variable and without ever exceeding the value of `CONFIG_REDIS_RETRY_DELAY_MAX`.
*   **`CONFIG_REDIS_RETRY_DELAY_MAX`** default(`60000`—Maximum time between connection retries to the real-time database.
