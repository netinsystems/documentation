# Anschriften

Das Unternehmen `addresses`innerhalb von ***NetinDS DriverAPI***ermöglicht den Zugriff auf die dem Piloten zugewiesenen Adresseneinstellungen. Jeder Pilot wird mit der Artefakt-ID identifiziert (`artifactId`) in der Initiierung der Instanz **DriverAPI**. Während des Prozesses der *enroll* des **DriverAPI** im System **Netinds**, dieser fordert alle *Herkunft* die von diesem Piloten zu erwerbenden Datenpunkte enthalten. **Netinds** diese Assoziation durch den Vermittler des Eigentums `originType` des `addressConfig` im Gerätemodell.

Die Modelle enthalten wiederum das Eigentum `originTypes`, der Typ `array`, die die angegebenen Parameter für diese Art von Gerät für jeden Treiber speichert. Diese Konfigurationen werden an einem Objekt des Typs [`DriverConfig<T>`](#interfaz-driverconfigt) von jedem Piloten. [`DriverConfig<T>`](#interfaz-driverconfigt) ein einziges obligatorisches Eigentum hat, `originType`, die als Kennung des Piloten dient (`artifactId`).

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

Jeder Pilot in **Netinds** a spezifische Merkmale, die von der Umsetzung abhängen, des nachgeschalteten Protokolls des **DriverAPI** und/oder arten von Geräten oder Systemen, die zur Erfassung von Informationen zu verwenden sind. Um diesen Eigenschaften gerecht zu werden, wird der config `addressConfig` es ermöglicht, die für die Anzahl der Nutzungsfälle erforderlichen Eigenschaften zu definieren, d. h. der Schöpfer eines neuen Treibers kann durch diese *config* Standard, bestimmen, wie sich der Pilot beim Zugriff auf Informationen verhalten soll.

![NetinDS-Config](../../.assets/address-relationship.png)

Es geht also um die `addressConfig`in den Modellen der **Netinds**, und die entsprechenden Schnittstellen [**`Datapoint`**](#interfaz-datapoint) y [**`DatapointItem`**](#interfaz-datapointitem)auf der Seite des **DriverAPI**, wie der Entwickler eines neuen Treibers definieren kann, wie sich sein Treiber verhalten soll. Alle Eigenschaften dieser Schnittstellen werden nur vom jeweiligen Treiber interpretiert.

Weitere Informationen zu den Modellen und deren Konfiguration in **Netinds** siehe die [Dokumentation](https://docs.netin.io/netin_webUI/NetinDS/templates).

## Inhaltsverzeichnis

*   [Anschriften](#addresses)
    *   [Inhaltsverzeichnis](#tabla-de-contenidos)
    *   [Schnittstelle: **`Origins<T>`**](#interfaz-originst)
        *   [Beispiel **`Origins<T>`**](#ejemplo-originst)
    *   [Schnittstelle: **`OriginsArray<T>`**](#interfaz-originsarrayt)
        *   [Beispiel **`OriginsArray<T>`**](#ejemplo-originsarrayt)
    *   [Schnittstelle: **`Origin<T>`**](#interfaz-origint)
    *   [Schnittstelle: **`OriginItem<T>`**](#interfaz-originitemt)
    *   [Schnittstelle: **`DriverConfig<T>`**](#interfaz-driverconfigt)
    *   [Schnittstelle: **`DatapointSet`**](#interfaz-datapointset)
    *   [Schnittstelle: **`DatapointSetItem`**](#interfaz-datapointsetitem)
    *   [Schnittstelle: **`DatapointSetAddress`**](#interfaz-datapointsetaddress)
    *   [Schnittstelle: **`Datapoint`**](#interfaz-datapoint)
    *   [Schnittstelle: **`DatapointItem`**](#interfaz-datapointitem)
    *   [Methode: **`getDatapoint`**](#método-getdatapoint)
    *   [Methode: **`getDatapointList`**](#método-getdatapointlist)
    *   [Methode: **`getDatapointSet`**](#método-getdatapointset)
    *   [Methode: **`getOrigin`**](#método-getorigin)
    *   [Methode: **`getOriginConfig`**](#método-getoriginconfig)
    *   [Methode: **`getOrigins`**](#método-getorigins)
    *   [Methode: **`getOriginsArray`**](#método-getoriginsarray)
    *   [Methode: **`hasDatapoint`**](#método-hasdatapoint)
    *   [Methode: **`hasDatapointSet`**](#método-hasdatapointset)
    *   [Methode: **`hasOrigin`**](#método-hasorigin)

***

## Schnittstelle: **`Origins<T>`**

Die Schnittstelle `Origins<T>` legt die Gesamtzahl der *Herkunft* für einen Piloten in Form eines Objekts (*Schlüsselwert*) jeder Schlüssel ist ein Identifikator von *Herkunft* und jeder Wert ein Objekt des Typs [`Origin<T>`](#interfaz-origint).

| Name| Typ | Defekt | Beschreibung |
| :----------------- | :------------------------------- | :------ | :---------------------------------------------------------------------------------- |
| `[origin: string]` | [`Origin<T>`](#interfaz-origint) |         | Gegenstand, der die Adressierung im Zusammenhang mit **Netinds** eines *Herkunft*. |

### Beispiel **`Origins<T>`**

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

## Schnittstelle: **`OriginsArray<T>`**

Die Schnittstelle `OriginsArray<T>` legt die Gesamtzahl der *Herkunft* für einen arrayförmigen Treiber, wobei jedes Element der Tabelle ein Objekt des Typs [`OriginItem<T>`](#interfaz-originitemt).

```typescript
type OriginsArray<T> = OriginItem<T>[];
```

### Beispiel **`OriginsArray<T>`**

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

## Schnittstelle: **`Origin<T>`**

Die Schnittstelle `Origin<T>` definiert die Adressierung im Kontext von **Netinds** eines *Herkunft*.

| Name| Typ | Defekt | Beschreibung |
| :------------------------- | :------------------------------------------- | :------ | :---------------------------------------------------------------------------------- |
| `driverConfig`             | [`DriverConfig<T>`](#interfaz-driverconfigt) |         | Objekt, das die Konfiguration des Treibers für die *Herkunft*.                     |
| `[datapointSetId: string]` | [`DatapointSet`](#interfaz-datapointset)     |         | Gegenstand zur Festlegung der Adressierung eines *DatapointSet* in einem *Herkunft* Beton. |

***

## Schnittstelle: **`OriginItem<T>`**

Die Schnittstelle `OriginItem<T>` definiert die Adressierung im Kontext von **Netinds** eines *Herkunft*.

| Name| Typ | Defekt | Beschreibung |
| :-------------- | :------------------------------------------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `origin`        | `string`                                           |         | Identifikator des *Herkunft*.                                                                                                                                     |
| `driverConfig`  | [`DriverConfig<T>`](#interfaz-driverconfigt)       |         | Objekt, das die Konfiguration des Treibers für die *Herkunft*.                                                                                                 |
| `datapointSets` | [`DatapointSetItem[]`](#interfaz-datapointsetitem) |         | Tabelle von Objekten des Typs [`DatapointSetItem`](#interfaz-datapointsetitem) die die Adressierung eines *DatapointSet* in einem *Herkunft* Beton. |

***

## Schnittstelle: **`DriverConfig<T>`**

Die Schnittstelle `DriverConfig<T>` ist die Union zwischen `T`, Schnittstelle definiert durch den Schöpfer des Treibers, und die Eigenschaft `driverId`:

```typescript
export type DriverConfig<T> = T & { driverId: string };
```

Dann für einen Treiber, bei dem der Schöpfer die folgende Schnittstelle definiert:

```typescript
interface myDriverConfig {
  address: string;
  myConfigObject: {
    importantProperty: number;
    otherImportantProperty: string[];
  };
}
```

Die kombinierte Schnittstelle wäre:

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

Und ein möglicher Wert davon:

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

## Schnittstelle: **`DatapointSet`**

Die Schnittstelle `DatapointSet` definiert die Adressierung eines *DatapointSet* in einem *Herkunft* Beton.

| Name| Typ | Defekt | Beschreibung |
| :---------------------- | :----------------------------------------------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------- |
| `type`                  | `DatapointSetType`                                     |         | Gibt die Art des DatapointSet an: `map` | `tableStatic` | `tableDynamic`                                                                |
| `address`               | [`DatapointSetAddress`](#interfaz-datapointsetaddress) |         | Gegenstand der Adressierung auf der Ebene von *DatapointSet* eines Tischtyps,`tableStatic` | `tableDynamic`).                                 |
| `[datapointId: string]` | [`Datapoint`](#interfaz-datapoint)                     |         | Gegenstand des Typs [`Datapoint`](#interfaz-datapoint) die die Adressierung eines Datenpunkts innerhalb eines *DatapointSet* konkret|

***

## Schnittstelle: **`DatapointSetItem`**

Die Schnittstelle `DatapointSetItem` definiert die Adressierung eines *DatapointSet* in einem *Herkunft* Beton.

| Name| Typ | Defekt | Beschreibung |
| :--------------- | :----------------------------------------------------- | :------ | :-------------------------------------------------------------------------------------------------------------------------------------------------- |
| `datapointSetId` | `string`                                               |         | Identifikator des *DatapointSet*                                                                                                                    |
| `type`           | `DatapointSetType`                                     |         | Geben Sie die Art der *DatapointSet*: `map` | `tableStatic` | `tableDynamic`                                                                          |
| `address`        | [`DatapointSetAddress`](#interfaz-datapointsetaddress) |         | `optional` Gegenstand zur Festlegung der Adressierung der Tabellen (`tableStatic` | `tableDynamic`).                                                       |
| `datapoints`     | [`DatapointItem[]`](#interfaz-datapointitem)           |         | Tabelle von Objekten des Typs [`DatapointItem`](#interfaz-datapoint) die die Adressierung eines Datenpunktes in einem bestimmten DatapointSet |

***

## Schnittstelle: **`DatapointSetAddress`**

Die Schnittstelle `DatapointSetAddress` setzt die Adressierung von Tabellen in Tabellen-Datapoints (`tableStatic` | `tableDynamic`). Ermöglicht es, eine globale Adresse für die Tabellen und Indizes dieser Tabellen zu definieren.

Dieses Merkmal der *DatapointSet* Tabelle-Typ ermöglicht es dem Schöpfer eines neuen Treibers, eine zusätzliche Adressierungsebene hinzuzufügen, mit der gleichen Flexibilität wie das Feld `address` Schnittstellen [**`Datapoint`**](#interfaz-datapoint) y [**`DatapointItem`**](#interfaz-datapointitem)für spezifische Anwendungsfälle.

Hier einige Beispiele:

*   Diese Adresse wird im Treiber verwendet. `netin-ds-drv-snmp`um die Adresse einer SNMP-Tabelle zu definieren.
*   Es wird durch die Implementierung von SQL-Treibern verwendet, um die Tabelle zu definieren, auf der die Abfrage durchgeführt wird, und unter Verwendung des Feldes `address`des Datenpunkts als Indikator für die Spalte.
*   ...

Für weitere Informationen siehe die [Dokumentation](https://docs.netin.io/netin_webUI/NetinDS/templates).

| Name| Typ | Defekt | Beschreibung |
| :------------ | :-------- | :------ | :------------------------------------------ |
| `rootAddress` | `string`  |         | Adresse in der Tabelle des DatapointSet |
| `indexes`     | `sting[]` |         | Liste der Indizes der Tabelle |

***

## Schnittstelle: **`Datapoint`**

Die Schnittstelle `Datapoint` legt die Adresse eines Datenpunkts fest.

| Name| Typ | Defekt | Beschreibung |
| :------------- | :-------------- | :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `type`         | `DatapointType` |         | Art des Datenpunktes, gibt an, wie der Datenpunkt im System verarbeitet wird **Netinds**. Lesen Sie die Dokumentation unter [Modelle](https://docs.netin.io/netin_webUI/NetinDS/templates) Von Netin für weitere Informationen. |
| `dataType`     | `DataType`      |         | Art des Feldes `value`/`rawValue` in der Veröffentlichung dieses Datenpunkts erwartet. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab.                                                                   |
| `rawDataType`  | `string`        |         | Ursprünglicher Datentyp. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab. <br> ***Format***: `/^[a-zA-Z0-9-_]{1,80}$/`                                                                          |
| `address`      | `string`        |         | Adresse der ursprünglichen Daten. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab. <br> ***Format***: `/^[\w\d_&#:$.-]{1,80}$/`                                                              |
| `accessMode`   | `AccessType`    |         | Umfang des Zugriffs auf die Daten, die sie ursprünglich erhalten haben. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab.                                                                                                             |
| `receiveMode`  | `ReceiveMode`   |         | Art des Dateneingangs. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab.                                                                                                                        |
| `pollingGroup` | `PollingGroup`  |         | Umfragegruppe, für `receiveMode: "polling"`. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab.                                                                                                  |

***

## Schnittstelle: **`DatapointItem`**

Die Schnittstelle `DatapointIem` legt die Adresse eines Datenpunkts fest.

| Name| Typ | Defekt | Beschreibung |
| :------------- | :-------------- | :------ | :----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `datapointId`  | `string`        |         | Datenpunkt-Idfiator |
| `type`         | `DatapointType` |         | Art des Datenpunktes, gibt an, wie der Datenpunkt im System verarbeitet wird **Netinds**. Lesen Sie die Dokumentation unter [Modelle](https://docs.netin.io/netin_webUI/NetinDS/templates) Von Netin für weitere Informationen. |
| `dataType`     | `DataType`      |         | Art des Feldes `value`/`rawValue` in der Veröffentlichung dieses Datenpunkts erwartet. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab.                                                                   |
| `rawDataType`  | `string`        |         | Ursprünglicher Datentyp. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab. <br> ***Format***: `/^[a-zA-Z0-9-_]{1,80}$/`                                                                          |
| `address`      | `string`        |         | Adresse der ursprünglichen Daten. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab. <br> ***Format***: `/^[\w\d_&#:$.-]{1,80}$/`                                                              |
| `accessMode`   | `AccessType`    |         | Umfang des Zugriffs auf die Daten, die sie ursprünglich erhalten haben. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab.                                                                                                             |
| `receiveMode`  | `ReceiveMode`   |         | Art des Dateneingangs. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab.                                                                                                                        |
| `pollingGroup` | `PollingGroup`  |         | Umfragegruppe, für `receiveMode: "polling"`. Dieser Bereich ist informativ, und seine Nutzung hängt von der Umsetzung des Piloten ab.                                                                                                  |

***

## Methode: **`getDatapoint`**

Gibt die Parameter des gewünschten Datenpunkts zurück.

```typescript
getDatapoint(origin: string, datapointSetId: string, datapointId: string): Datapoint | undefined
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :--------------- | :------- | :---------------------- |
    | `origin`         | *Seil* | Ursprung beantragt |
    | `datapointSetId` | *Seil* | DatapointSet angefordert |
    | `datapointId`    | *Seil* | Datapoint gefragt |

    **Rücksendungen:** [`Datapoint`](#interfaz-datapoint) | `undefined`

***

## Methode: **`getDatapointList`**

```typescript
getDatapointList(origin: string, datapointSet: string): string[]
```

Gibt die Liste der Datenpunkte zurück, aus denen ein bestimmtes DatapointSet in einer bestimmten Quelle besteht.

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------------- | :------- | :---------------------- |
    | `origin`       | *Seil* | Ursprung beantragt |
    | `datapointSet` | *Seil* | DatapointSet angefordert |

    **Rücksendungen:** `string[]`

***

## Methode: **`getDatapointSet`**

Gibt die Konfiguration eines bestimmten DatapointSet in einer bestimmten Quelle zurück

```typescript
getDatapointSet(origin: string, datapointSetId: string): DatapointSet | undefined
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :--------------- | :------- | :---------------------- |
    | `origin`         | *Seil* | Ursprung beantragt |
    | `datapointSetId` | *Seil* | DatapointSet angefordert |

    **Rücksendungen:** [`DatapointSet`](#interfaz-datapointset) | `undefined`

***

## Methode: **`getOrigin`**

Gibt die Konfiguration einer bestimmten Quelle zurück.

```typescript
getOrigin<T>(origin: string): Origin<T> | undefined
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------- | :------- | :---------------- |
    | `origin` | *Seil* | Ursprung beantragt |

    **Rücksendungen:** [`Origin<T>`](#interfaz-origint) | `undefined`

***

## Methode: **`getOriginConfig`**

Gibt die Konfiguration des Treibers für eine bestimmte Quelle zurück.

```typescript
getOriginConfig<T>(origin: string): DriverConfig<T> | undefined
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------- | :------- | :---------------- |
    | `origin` | *Seil* | Ursprung beantragt |

    **Rücksendungen:** [`DriverConfig<T>`](#interfaz-driverconfigt) | `undefined`

***

## Methode: **`getOrigins`**

Gibt die Konfiguration aller Systemquellen für den Treiber an das Objektformat (JSON) zurück.

```typescript
getOrigins<T>(): Origins<T>
```

**Rücksendungen:** [`Origins<T>`](#interfaz-originst)

***

## Methode: **`getOriginsArray`**

Gibt die Konfiguration aller Systemquellen für den Treiber an das Array-Format zurück.

```typescript
getOriginsArray<T>(): OriginsArray<T>
```

**Rücksendungen:** [`OriginsArray<T>`](#interfaz-originsArrayt)

***

## Methode: **`hasDatapoint`**

Überprüfen Sie, ob ein spezieller Datenpunkt in der Konfiguration vorhanden ist.

```typescript
hasDatapoint(origin: string, datapointSetId: string, datapointId: string): boolean
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :--------------- | :------- | :---------------------- |
    | `origin`         | *Seil* | Ursprung beantragt |
    | `datapointSetId` | *Seil* | DatapointSet angefordert |
    | `datapointId`    | *Seil* | Datapoint gefragt |

    **Rücksendungen:** `boolean`

***

## Methode: **`hasDatapointSet`**

Überprüfen Sie, ob ein bestimmtes DatapointSet in der Konfiguration vorhanden ist.

```typescript
hasDatapointSet(origin: string, datapointSet: string): boolean
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------------- | :------- | :---------------------- |
    | `origin`       | *Seil* | Ursprung beantragt |
    | `datapointSet` | *Seil* | DatapointSet angefordert |

    **Rücksendungen:** `boolean`

***

## Methode: **`hasOrigin`**

Prüft, ob eine bestimmte Quelle in der Konfiguration vorhanden ist.

```typescript
hasOrigin(origin: string): boolean
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------- | :------- | :---------------- |
    | `origin` | *Seil* | Ursprung beantragt |

    **Rücksendungen:** `boolean`
