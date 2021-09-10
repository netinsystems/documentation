# @netin-js/nds-driver-api

***

## Inhaltsverzeichnis

*   [Einführung](#introducción)
*   [Installation](#instalación)
*   [Informationen](#información)
*   [Nutzung](#uso)

## Einführung

Das Ziel von **NetinDS DriverAPI** die Entwicklung von "Treibern" zu erleichtern, die die Integration neuer Informationsquellen ermöglichen. Innerhalb des Ökosystems **Netin-Systeme**die "Treiber" für die Erfassung und Veröffentlichung von Informationen verantwortlich sind (*die Daten* y *Die Termine*) und Veranstaltungen (*Alarmanlagen*) aus den Feldelementen ([Ladenboden](https://en.wikipedia.org/wiki/Shop_floor)) und dritte Systeme zum Kern der Agenten **Netinds**.

![NetinDS-Architecture](../.assets/netinds-architecture.png)

**NetinDS DriverAPI** ermöglicht:

*   Arbeiten zur Veröffentlichung von Datenpunktstrukturen im Karten- und Tabellenformat ausführen.
*   Arbeiten zur Veröffentlichung von Zeitreihen (Timepoints) ausführen.
*   Durchführung von Benachrichtigungen über Ereignisse und Alarme.

## Installation

**NetinDS DriverAPI** Jeder Node.js®-kompatible Paketmanager ist installiert.

*   **npm**

```sh
npm install @netin-js/nds-drv-api
```

*   **yarn**

```sh
yarn add @netin-js/nds-drv-api
```

## Informationen

Die folgenden Schritte führen Sie durch den Prozess der Erstellung eines Gerätemodells, das Ihren neuen Treiber und konfiguration sogar für die Veröffentlichung von Ereignissen und Informationen über ein System verwendet. **Netinds**.

> **Um den Prozess zu vervollständigen, muss ein NetinDS-System mit mindestens einem einsatzbereiten Agenten vorhanden sein.**

### **Schritt 1: Generation eines Gerätemodells**

Hier ist das Modell (im YAML-Format), das als Grundlage für das in diesen Schritten entwickelte Beispiel dienen wird. Dieses Modell wird einige Datenpunkte deklarieren, die von unserem Piloten veröffentlicht werden müssen, mit denen wir die `originType: 'custom-dvr'`.

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

Weitere Informationen über die Erstellung eines Modells finden Sie in der [Dokumentation](https://docs.netin.io/).

### **Schritt 2: Laden des Gerätemodells auf ein NetinDS-System (Dragonfly)**

Um diesen Schritt abzuschließen, befolgen Sie die Anweisungen der [Dokumentation](https://docs.netin.io/netin_webUI/NetinDS/standalone/settings-menu/standalone-repository/#plantillas) von **Netinds**.

### **Schritt 3: Registrierungsprozess des Piloten**

Mit dem auf unser System geladenen Modell **Netinds** Wir können damit beginnen, unseren neuen Piloten zu programmieren. Generell wird empfohlen, folgende Schritte zu befolgen:

*   Eine Instanz für unseren Piloten schaffen `'custom-drv'`.
*   Abonnieren Sie die Ereignisse in unserer Treiber-Instanz, damit Sie Stream-Updates, Fehler und Statusänderungen verwalten können.
*   Fragen Sie den Abonnement-Prozess mit dem **NetinDS Agent**.

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

### **Schritt 4: Adressverwaltung unseres Treibers**

Sobald unsere Instanz abonniert wird, und nach Erhalt der Veranstaltung `updated`haben wir Zugriff auf die Konfiguration unseres Treibers. Hier ist ein Beispiel, wie man dieses Management in folgenden Schritten durchführen kann:

*   Erklärung der konfigurationsspezifischen Schnittstelle für unseren Treiber `custom-drv`.
*   Zugriff auf die Konfiguration aller Ursprünge im Tabellenformat.
*   Zugriff auf die Konfiguration aller Ursprünge im Objektformat.

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

Die Ausführung des angegebenen Codes auf einem System, bei dem es nur ein Team gab, auf dem wir Daten sammeln wollen, und dessen `ICMP_IPAddress` nach außen `192.168.1.1`, wird das folgende Ergebnis

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

### **Schritt 5: Veröffentlichung und Mitteilung**

Mit unserem Treiber abonniert **NetinDS Agent** und jetzt, da wir die Konfiguration von *Herkunft*, *Datenpunktsätze* y *die Daten* Wir hoffen zu lesen, können wir dazu übergehen, Informationen zu veröffentlichen und Ereignisse zu melden.

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

> Beachten Sie, dass kein Unterschied erforderlich ist. *DIE DATAPOINTS* von *LES TIMEPOINTS*, es ist etwas, das **DriverAPI** wird für uns unter Berücksichtigung der Konfiguration des Modells.

## Nutzung

### Pilot mit Informationsspitzen und Belegung des *Datenpfad*

Wenn Ihr Treiber viele Informationen generiert, haben Sie möglicherweise einige Probleme zu bewältigen:

*   Veröffentlichung von *DIE DATAPOINTS* in der *Datenpfad*: der *Datenpfad* von **Netinds** funktioniert wie eine *Schwanz*, am *Kern* des **NetinDS Agent** Es ist für den Verbrauch der Daten zuständig, wenn sie die Warteschlangen der Veröffentlichung erreichen, um zu verhindern, dass ein Pilot das System überlastet, es gibt einen inhärenten Systemschutz: Wenn die zu veröffentlichenden Daten noch in der Warteschlange vorhanden sind, werden sie nicht veröffentlicht und die Arbeit endet mit einem Fehler.
*   Schaffung einer großen Anzahl von Arbeiten: Standardmäßig ist der maximale Wert der parallelen Arbeiten, die erstellt werden können, `100`, wenn Sie diese Anzahl von Arbeiten erhöhen müssen, um mit Informationsspitzen umgehen zu können, wo eine große Anzahl von Arbeiten veröffentlicht werden müssen, können Sie dies mit der Umgebungsvariable tun `CONFIG_STREAM_HIGH_WATER_MARK`. Vergessen Sie nicht, dass durch die Erhöhung dieses Wertes der Ressourcenverbrauch des Knotens steigen würde.

### Hinzufügen von Informationen von unserem Treiber zur Beobachtung von NetinDS

**DriverAPI** ermöglicht die Veröffentlichung von Informationen, die für den internen Zustand unseres Treibers relevant sind, auf der Beobachtungsschnittstelle von **Netinds**, dazu können wir die Methoden [`addFailure`](./docs/Driver.md#método-addfailure) y [`updateSubcomponent`](./docs/Driver.md#método-updatesubcomponent).

```typescript
// Publicación de fallos
driver.addFailure({
  errors: [{ error: `My error text`, source: `The Source` }],
});

//Publicación de subcomponentes
driver.updateSubcomponent({ status: 'warn', componentId: 'myComponent' });
```

Sie können den Status Ihres Treibers erwerben, einschließlich der Veröffentlichung Ihrer eigenen Fehler und Komponenten an den Endpunkten:

*   `http://127.0.0.1:{PORT}/v1/health`
*   `http://127.0.0.1:{PORT}/v1/registers`

> `PORT` wird den Wert der Immobilie haben `healthPort`, dem Hersteller zugewiesen **DriverAPI**. Wenn diese Eigenschaft initialisiert wurde, wird der Wert der Umgebungsvariable sein. `CONFIG_SERVER_PORT`.

### Wiederholungen des Abonnementprozesses

Um sicherzustellen, dass unser Pilot den Abo-Prozess beim Start des Systems ordnungsgemäß durchführt, ist es üblich, einen Algorithmus zu implementieren, der es ermöglicht, diese Operation auf unbestimmte Zeit zu wiederholen, bis sie durchgeführt ist.

Hier ist ein Beispiel für einen Code mit der Implementierung von Versuchen, die das Intervall bis zu einem gewissen Maximum erhöhen, indem sie zwei Hilfsfunktionen verwenden.

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

*   [Pilot](./@netin%2Djs-%2D-Driver%2DAPI/Driver.md)
*   [Editor](./@netin%2Djs-%2D-Driver%2DAPI/Publisher.md)
*   [Mitteilung](./@netin%2Djs-%2D-Driver%2DAPI/Notifier.md)
*   [Anschriften](./@netin%2Djs-%2D-Driver%2DAPI/Addresses.md)
*   [Alarmanlagen](./@netin%2Djs-%2D-Driver%2DAPI/Alarms.md)

### Umweltvariablen

#### Allgemeine Einstellungen netinDS-DriverAPI

*   **`CONFIG_ENTITY_ID`** standardmäßig,`uninitialized`UUID (RFC4122) **NetinDS Agent** für die dieses Gremium von **DriverAPI**. Ursprünglich ist es nicht notwendig, diesen Wert zu konfigurieren, da er in der Regel vom Entdeckungsdienst angegeben wird:
    *   Im geerbten Modus, d. h. im Modus **NetinDS Dragonfly** Dieser Kennung wird aus der Konfigurationsdatenbank ausgewählt.
    *   Im Jahr **Spinne Netinds** Diese Kennung wird vom Entdeckungsdienst angegeben.
*   **`CONFIG_ARTIFACT_RELEASE`** standardmäßig,`0.0.0`) legt die zu verwendenden Versionen und Versionen des **DriverAPI** im Zusammenhang mit der Beobachtbarkeit von **Netinds**.
*   **`CONFIG_ENABLED_PERIOD_COT`** standardmäßig,`false`) aktiviert den Modus des Sendens von Alarmen mit `CoT`(*Ursache für die Übertragung*) `Period`, d. h. dass der Server bei jedem Bewertungsereignis über den Status eines Alarms informiert wird.
    > ***Die Verwendung wird nicht empfohlen, außer in besonderen Fällen, in denen eine ständige Überprüfung des Alarmzustands erforderlich ist.***
*   **`CONFIG_STREAM_HIGH_WATER_MARK`** standardmäßig,`100`) gibt die Größe der internen Puffer von **DriverAPI** die für backpressure-Funktionen verwendet werden. **DriverAPI** verfügt über mehrere Behandlungspuffer, so dass die Gesamtgröße des Puffers von der Summe von allen abhängig ist, und die Art der zu verarbeitenden Daten.
    > ***Der Zweck dieses Puffers ist es, sich stabil an die Datenspitzen anzupassen, ohne übermäßigen Ressourcenverbrauch, darf dieser alternative Puffer nicht zu den Puffern der Firehorse verwendet werden.***
*   **`CONFIG_SERVER_PORT`** standardmäßig,`3000`): Der Port, auf dem die REST API für die Beobachtbarkeit von **Netinds**dieser Wert nur angewendet wird, wenn die Eigenschaft nicht initialisiert wurde `healthPort` im Hersteller des **DriverAPI**. Diese REST API wird an den Endpunkten angeboten:
    *   `http://127.0.0.1:{PORT}/v1/alarms`: Aufzeichnung von Alarmanlagen, die seit **DriverAPI** an den Server von **Netinds** Und seinen Zustand.
    *   `http://127.0.0.1:{PORT}/v1/halted-origins`: Aufzeichnung der Quellen, die beim Start von **DriverAPI** um Fehler zu enthalten.
    *   `http://127.0.0.1:{PORT}/v1/health`: Zustand der internen Teilkomponenten von **DriverAPI** .
    *   `http://127.0.0.1:{PORT}/v1/metrics`: Maßnahmen des **DriverAPI** im Format [prometheus](https://prometheus.io/).
    *   `http://127.0.0.1:{PORT}/v1/quarantine`: Aufzeichnung von DatapointSet oder Datapoints in Quarantäne, d. h. von Datenpunkten, deren Konfiguration nicht korrekt ist.
    *   `http://127.0.0.1:{PORT}/v1/registers`: Aufzeichnung der letzten Veröffentlichungsarbeiten an den Server, der einen Fehler beendet hat. Die Größe dieser Liste kann mit hilfe der Umgebungsvariable konfiguriert werden: `CONFIG_CONFLICT_BUFFER_LENGTH`, deren Standardwert `100`.
        > ***Änderung der nicht empfohlenen Variable `CONFIG_CONFLICT_BUFFER_LENGTH` Wenn wir nicht wissen, was wir tun.***
*   **`CONFIG_SERVER_HOST`** standardmäßig,`localhost`) IP-Adresse, mit der die REST-Api für Beobachtbarkeit verbunden ist.
*   **`CONFIG_SERVER_MODE`** standardmäßig,`http`): Die Betriebsart der REST API der Beobachtbarkeit. Die gültigen Optionen sind `http` o `https`dies erfordert die Konfiguration der Zertifikate für den Server.
*   **`CONFIG_SERVER_FILE_CERT_PATH`** standardmäßig,`certs/cert.pem`Pfad zur Zertifikatsdatei für den Server. Nur im Modus verwendet `https`.
*   **`CONFIG_SERVER_FILE_KEY_PATH`** standardmäßig,`certs/key.pem`) Zugang zu der privaten Schlüsseldatei. Nur im Modus verwendet `https`.

#### Einstellungen des Entdeckungsdienstes

Der Entdeckungsdienst ist für die Verwaltung von Geräten/Systemen verantwortlich, die von einem **Netinds**.

*   **`CONFIG_DISCOVERY_HTTP_ADDR`** standardmäßig,`http://127.0.0.1:29800`URL des Entdeckungsdienstes.
    > ***Wenn diese Variable nicht definiert ist, wird das System im geerbten Modus, d. h. im DragonFly-NetinDS-Modus, arbeiten und die Konfiguration direkt an die Datenbank anfordern.***
*   **`CONFIG_DISCOVERY_ADDRESSES_HTTP_PATH`** standardmäßig,`/api/v1/addresses`) Api-Pfad des Entdeckungsdienstes, um 'Adressen' von Geräten/Systemen zu erhalten.
*   **`CONFIG_DISCOVERY_RESOURCES_HTTP_PATH`** standardmäßig,`/api/v1/resources`): Pfad der API des Entdeckungsdienstes zur Beschaffung von Ressourcen (globale Variablen, Konfigurationsdateien ...).
*   **`CONFIG_DISCOVERY_ENROLL_HTTP_PATH`** standardmäßig,`/api/v1/enroll`) Api-Pfad des Entdeckungsdienstes für die Anmeldung neuer Geräte oder Systeme.
*   **`CONFIG_DISCOVERY_UPDATE_INTERVAL`** standardmäßig,`60000`) Gibt den Zeitintervall für die Überprüfung von Aktualisierungen in der Systemkonfiguration an.
*   **`CONFIG_DISCOVERY_OPEN_TIMEOUT`** standardmäßig,`2000`) maximale Wartezeit für die Anschlussanbindung.
*   **`CONFIG_DISCOVERY_RESPONSE_TIMEOUT`** standardmäßig,`5000`) Maximale Reaktionszeit des Entdeckungsdienstes im Verbindungsprozess. Diese Zeit beginnt zu zählen, seit die Verbindung geöffnet ist.
*   **`CONFIG_DISCOVERY_READ_TIMEOUT`** standardmäßig,`5000`) maximale Frist für den Eingang der Antwort auf Leseanfragen.
*   **`CONFIG_DISCOVERY_AUTO_UPDATE`** standardmäßig,`false`) Gibt dem System an, mit automatischen Updates zu arbeiten, d. h. nicht auf die Bestätigung warten, die es aktualisieren muss.
    > ***Es wird nicht empfohlen, den Standardwert zu ändern, wenn nicht genau bekannt ist, was getan wird.***
*   **`CONFIG_DISCOVERY_LAZY_START`** standardmäßig,`false`) gibt dem System an, dass es sich beim Start mit dem Entdeckungsdienst verbinden muss.
    > ***Es wird nicht empfohlen, den Standardwert zu ändern, wenn nicht genau bekannt ist, was getan wird.***
*   **`CONFIG_DISCOVERY_HTTP_TOKEN`** standardmäßig,`false`) Zugang zur API des Entdeckungsdienstes, erforderlich, wenn die Zugriffskontrolle aktiviert ist.
*   **`CONFIG_DISCOVERY_HTTP_SSL`** standardmäßig,`false`) die Verwendung des HTTPS-Kommunikationsschemas ermöglicht.
*   **`CONFIG_DISCOVERY_HTTP_SSL_VERIFY`** standardmäßig,`true`) ermöglicht die Überprüfung der Bescheinigungen des Entdeckungsservers, wenn `CONFIG_DISCOVERY_HTTP_SSL_VERIFY`Sie ist aktiviert.
*   **`CONFIG_DISCOVERY_CA_PATH`** standardmäßig,`undefined`): Pfad zur CA-Zertifikatsdatei des Entdeckungsdienstes. Nur verwendet, wenn `CONFIG_DISCOVERY_HTTP_SSL` Dieses Vermögen.
*   **`CONFIG_DISCOVERY_CLIENT_CERT_PATH`** standardmäßig,`undefined`) Pfad für den Zugriff auf die Client-Zertifikat-Datei benötigt, um auf den Entdeckungsservice zuzugreifen. Nur verwendet, wenn `CONFIG_DISCOVERY_HTTP_SSL` Dieses Vermögen.
*   **`CONFIG_DISCOVERY_CLIENT_KEY_PATH`** standardmäßig,`undefined`) Pfad des Zugriffs auf die Schlüsseldatei des Kundenzertifikats, die für den Zugriff auf den Entdeckungsservice erforderlich ist. Nur verwendet, wenn `CONFIG_DISCOVERY_HTTP_SSL` Dieses Vermögen.
*   **`CONFIG_DISCOVERY_TLS_SERVER_NAME`** standardmäßig,`undefined`): Name des zu verwendenden Servers als [SNI](https://es.wikipedia.org/wiki/Server_Name_Indication) wenn die Verbindung in TLS auftritt. Nur verwendet, wenn `CONFIG_DISCOVERY_HTTP_SSL` Dieses Vermögen.

#### Einstellungen für den Zugriff auf die Konfigurationsdatenbank (Legacy)

*   **`CONFIG_MONGO_ADDR`** standardmäßig,`mongodb://127.0.0.1:28900/netin`) URL, in der sich die Konfigurationsdatenbank des Agenten befindet **Netinds**.
*   **`CONFIG_MONGO_SERVER_SELECTION_TIMEOUT`** standardmäßig,`5000`) Maximale Frist für die Auswahl eines Servers für die Durchführung einer Operation. Es wird nur verwendet, wenn die Konfigurationsdatenbank im Cluster-Modus funktioniert.
*   **`CONFIG_MONGO_POOL_SIZE`** standardmäßig,`4`) maximale Größe des Verbindungsstapels gegen die Datenbank.
*   **`CONFIG_MONGO_KEEP_ALIVE`** standardmäßig,`true`) aktiviert keepalive auf Verbindungen gegen die Datenbank und bleibt so etabliert.
*   **`CONFIG_MONGO_KEEP_ALIVE_INITIAL_DELAY`** standardmäßig,`10000`) Wartezeit vor dem ersten Keepalive auf den Verbindungen gegen die Datenbank.
*   **`CONFIG_MONGO_CONNECTION_TIMEOUT`** standardmäßig,`10000`) maximale Wartezeit bis zu einem Versuch, die Datenbank zu verbinden.
*   **`CONFIG_MONGO_SOCKET_TIMEOUT`** standardmäßig,`10000`) maximale Wartezeit für Anträge auf Übermittlung oder Empfang auf dem socket, das zur Verbindung mit der Datenbank verwendet wird.

#### Einstellungen für den Zugriff auf die Echtzeit-Datenbank (Redis)

*   **`CONFIG_REDIS_ADDR`** standardmäßig,`redis://127.0.0.1:28910/0`URL, wo sich die Echtzeit-Datenbank des Agenten von **Netinds**.
*   **`CONFIG_REDIS_PASSWORD`** standardmäßig,`undefined`) Passwort für den Zugriff auf die Datenbank in Echtzeit.
*   **`CONFIG_REDIS_KEEPALIVE`** standardmäßig,`10000`) Zeitintervall zwischen keepalive und Verbindungssockeln zur Echtzeit-Datenbank.
*   **`CONFIG_REDIS_LAZY_START`** standardmäßig,`true`) Gibt dem System an, dass es sich beim Start in Echtzeit mit der Datenbank verbinden muss.
    > ***Es wird nicht empfohlen, den Standardwert zu ändern, wenn nicht genau bekannt ist, was getan wird.***
*   **`CONFIG_REDIS_CONNECTION_TIMEOUT`** standardmäßig,`10000`) maximale Wartezeit bis zu einem Versuch, die Datenbank zu verbinden.
*   **`CONFIG_REDIS_RETRY_DELAY_FACTOR`** standardmäßig,`2000`) Zeit zwischen den Versuchen, die Datenbank in Echtzeit zu verbinden. Die Zeit wird bei jedem Ausfall schrittweise nach dem Wert dieser Variablen erhöht, ohne jemals den Wert von `CONFIG_REDIS_RETRY_DELAY_MAX`.
*   **`CONFIG_REDIS_RETRY_DELAY_MAX`** standardmäßig,`60000`) maximale Zeit zwischen den Versuchen, die Datenbank in Echtzeit zu verbinden.
