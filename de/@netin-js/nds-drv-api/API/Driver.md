# Pilot

Das Unternehmen `Driver`, ist die Haupteinheit des  ***NetinDS DriverAPI***, die es ermöglicht, eine neue Instanz des Gremiums zu schaffen. Diese Einheit ist der Hauptzugang zu anderen Methoden, Eigenschaften und Wesen.

## Inhaltsverzeichnis

*   [Pilot](#driver)
    *   [Inhaltsverzeichnis](#tabla-de-contenidos)
    *   [Schnittstelle: **`DriverOptions`**](#interfaz-driveroptions)
    *   [Schnittstelle: **`DriverHealthStatus`**](#interfaz-driverhealthstatus)
    *   [Schnittstelle: **`ComponentDetails`**](#interfaz-componentdetails)
    *   [Schnittstelle: **`SubcomponentDetail`**](#interfaz-subcomponentdetail)
    *   [Schnittstelle: **`JobResult`**](#interfaz-jobresult)
    *   [Schnittstelle: **`DriverFailure`**](#interfaz-driverfailure)
    *   [Enum: **`SubcomponentState`**](#enum-subcomponentstate)
    *   [Hersteller: **`Driver`**](#constructor-driver)
    *   [Eigentum: **`addresses`**](#propiedad-addresses)
    *   [Eigentum: **`alarms`**](#propiedad-alarms)
    *   [Eigentum: **`notifier`**](#propiedad-notifier)
    *   [Eigentum: **`publisher`**](#propiedad-publisher)
    *   [Eigentum: **`configErrors`**](#propiedad-configerrors)
    *   [Eigentum: **`configState`**](#propiedad-configstate)
    *   [Eigentum: **`hasConfigErrors`**](#propiedad-hasconfigerrors)
    *   [Eigentum: **`id`**](#propiedad-id)
    *   [Eigentum: **`isEnrolled`**](#propiedad-isenrolled)
    *   [Eigentum: **`state`**](#propiedad-state)
    *   [Eigentum: **`status`**](#propiedad-status)
    *   [Veranstaltung: **`done`**](#evento-done)
    *   [Veranstaltung: **`update`**](#evento-update)
    *   [Veranstaltung: **`updated`**](#evento-updated)
    *   [Veranstaltung: **`error`**](#evento-error)
    *   [Veranstaltung: **`state`**](#evento-state)
    *   [Methode: **`addFailure`**](#método-addfailure)
    *   [Methode: **`update`**](#método-update)
    *   [Methode: **`updateSubcomponent`**](#método-updatesubcomponent)
    *   [Methode: **`disenroll`**](#método-disenroll)
    *   [Methode: **`enroll`**](#método-enroll)

***

## Schnittstelle: **`DriverOptions`**

Die Schnittstelle `DriverOptions` legt die Konfigurationsoptionen für die Instanz **DriverAPI**.

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `artifactId` | `string` | | Treiberkennung, muss dem Wert der Eigenschaft entsprechen `originType` in den `addressConfig` der *die Daten* diesem Treiber zugeordnet und mit dem Konfigurationseingang des Piloten in der Eigenschaft `originTypes` der [*Modelle*](https://docs.netin.io/netin_webUI/NetinDS/templates/). |
| `updateInterval` | `number` | `60000` | `optional` Zeitintervall zwischen Den Überprüfungsanträgen für neue Konfigurationen. |
| `autoUpdate` | `boolean` | `false` | `optional` Automatische Aktualisierung, wenn der Treiber aktiviert ist, wird er keine Ereignisse ausstrahlen `update`, wird jedoch die Konfiguration aktualisieren, sobald sie verfügbar ist, indem die Ereignisse ausstrahlen `updated` später.  |
| `healthPort` | `number` | `3000` | `optional` Hafen, in dem **DriverAPI** die Beobachtbarkeitsschnittstelle von **Netinds** |
| `agentHostAddress` | `string` | `localhost`| `optional` Gastgeber, wo sich der **NetinDS Agent** |
| `discoveryHostAddress` | `string` | `localhost`| `optional` Gastgeber, wo sich der Service befindet **Entdeckung** des **Netin Agent**. Wenn sie konfiguriert ist, wird sie anstelle von `agentHostAddress` |
| `rdbHostAddress` | `string` | `localhost`| `optional` Gastgeber, wo sich der Service befindet **RDB** des **Netin Agent**. Wenn sie konfiguriert ist, wird sie anstelle von `agentHostAddress` |
| `semVer` | `string` | `0.0.0`| `optional` Version des Treibers im Semver-Format |

***

## Schnittstelle: **`DriverHealthStatus`**

Die Schnittstelle `DriverHealthStatus` gibt die relevanten Informationen über den Zustand des **DriverAPI**.

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `status` | `'pass' \| 'fail' \| 'warn'` | | Geben Sie die *Status* von **DriverAPI** aus der Sicht der *Beobachtbarkeit* von **Netinds**. |
| `description` | `string` | | `optional` Beschreibung der Instanz des **DriverAPI**. |
| `releaseID` | `string` | | `optional` Version im Semver-Format. Entspricht dem Wert der Immobilie `semVer`der Schnittstelle [`DriverOptions`](#interfaz-driveroptions) in der Initialisierung des Piloten oder der Umgebungsvariable angegeben `CONFIG_ARTIFACT_RELEASE`, gibt damit die Version des **DriverAPI**. |
| `version` | `string` | | `optional` Version des Treibers, vom Wert der Eigenschaft abgezogen `semVer`der Schnittstelle [`DriverOptions`](#interfaz-driveroptions) in der Initialisierung des Piloten oder der Umgebungsvariable angegeben `CONFIG_ARTIFACT_RELEASE`, wenn nicht, gibt die Fassung des **DriverAPI**. |
| `serviceId` | `string` | | Pilot *Kennung, die allgemein einzigartig ist* (*UUID*) entspricht dem Wert von [`id`](#propiedad-id). |
| `output` | `string` | | `optional` Weitere Informationen zum aktuellen Stand des **DriverAPI**. |
| `notes` | `string[]` | | `optional` Array mit relevanten Notizen über den aktuellen Status des Treibers **DriverAPI**. |
| `links` | `object` | | `optional` Gegenstand mit externen Links, die Informationen enthalten, die den aktuellen Stand des **DriverAPI**. |
| `details`| [`ComponentDetails`](#interfaz-componentdetails) | | `optional` Gegenstand mit der Darstellung des *Status* der verschiedenen Teilkomponenten, die die Instanz des **DriverAPI**. |

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

## Schnittstelle: **`ComponentDetails`**

Die Schnittstelle `ComponentDetails` gibt die Liste der Teilkomponenten an, die teil der Instanz des **DriverAPI**.

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `[subcomponent: string]` | `SubcomponentDetail[]` | | Jede Teilkomponente kann einen eigenen Eintrag in dieses Objekt haben. Jeder Eintrag kann aus einem oder mehreren Objekten des Typs bestehen. [`SubcomponentDetail`](#interfaz-subcomponentdetail). |

Standardmäßig ist jede Instanz von **Treiber-API** enthält folgende Unterbauteile:

*   `address:connectionState`: gibt den Stand der Verbindung mit dem Konfigurationsmanagementsystem an *Herkunft*. Diese Komponente ist nicht vorhanden, wenn die Unterkomponente `addressLegacy:connectionState` Es funktioniert.
*   `addressLegacy:connectionState`: gibt den Status der Verbindung zum System an *Vermächtnis* Verwaltung der Konfigurationen von *Herkunft*. Diese Komponente ist nicht vorhanden, wenn die Unterkomponente `address:connectionState` Es funktioniert.
*   `redis:connectionState`: gibt den *Status* der Verbindung mit der Redis-Instanz des **NetinDS Agent**.
*   `config:state`: gibt den *Status* die Konfiguration der Instanz **DriverAPI**.
*   `curator:component`: gibt den *Status* der Unterkomponente, die die Veröffentlichungs- und Benachrichtigungsarbeiten analysiert, um Fehler bei der Ausführung zu vermeiden.
*   `publisher:component`: gibt den *Status* Veröffentlichungs- und Benachrichtigungskomponente **Netinds**.
*   `uptime`: gibt die Ausführungszeit der Instanz an **DriverAPI**.

Neben den standardmäßigen Teilkomponenten gibt es eine Komponente namens `${artifactId}:external`, wo `artifactId` ist der Name unseres Treibers, bei dem der Treiber den *Status* die für die Anwendung der Methode geeigneten Elemente [`updateSubcomponent`](#método-updateSubcomponent).

***

## Schnittstelle: **`SubcomponentDetail`**

Die Schnittstelle `SubcomponentDetail` gibt die einschlägigen Informationen über das *Status* Teilkomponenten, die teil der Instanz des **DriverAPI**.

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `status` | `'pass' \| 'fail' \| 'warn'` | | Geben Sie die *Status* des Unterbestandteils unter dem Gesichtspunkt der *Beobachtbarkeit* von **Netinds**. |
| `componentId` | `string` | | Pilot *Kennung, die allgemein einzigartig ist* (*UUID*). |
| `componentType` | `string` | | In diesem Eintrag angegebene Unterbau- und Messart. |
| `links` | `object` | | `optional` Gegenstand mit externen Links, die Informationen über den aktuellen Zustand der Teilkomponente enthalten. |
| `metricUnit` | `string` | | `optional` Messeinheit für die in diesem Eintrag angegebene Messeinheit. |
| `metricValue` | `any` | | `optional` Wert der in diesem Eintrag angegebenen Messung. |
| `output` | `string` | | `optional` Weitere Informationen zum aktuellen Stand des **DriverAPI**. |
| `time` | `SubcomponentDetail` | | `optional` Datum, an dem der Zustand dieser Teilkomponente aktualisiert wird. |

***

## Schnittstelle: **`JobResult`**

Die Schnittstelle `JobResult` die relevanten Informationen über die Ausführung einer Veröffentlichungs- oder Notifizierungsarbeit enthält.

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `createdAt` | `string` | | Timestamp im ISO-Format, mit dem Datum der Erstellung der Arbeit. |
| `resolvedAt` | `string` | | Timestamp im ISO-Format, mit dem Datum der Arbeitsabwicklung. |
| `id` | `string` | | UUID der Arbeit. Diese eindeutige Kennung wird durch die Veröffentlichungs- und Meldefunktionen zurückgegeben, wenn eine neue Stelle beantragt wird. |
| `hasErrors` | `boolean` | `false` | Booleschen Wert, der anzeigt, ob die Arbeit mit Fehlern endet. |
| `errors` | `{ datapoint: string; error: string \| string[] }[]` | `[]` | `optional` Gemälde von Objekten. Das Eigentum `datapoint` von diesen Objekten angegeben *Datenpunkt* ó *DatapointSet* wo der Fehler aufgetreten ist, während die Eigenschaft `error` gibt die Fehler- oder Fehlerart an. |
| `quantity` | `{ alarms: number; datapoints: number; timepoints: number }` | | Anzahl der in diesem Dokument veröffentlichten Datapoints, Alarme/Ereignisse und Timepoints. |
| `type` | `'map' \| 'table' \| 'alarm'` | | Art der Arbeit |

***

## Schnittstelle: **`DriverFailure`**

Die Schnittstelle `DriverFailure` enthält die informationen, die für die Aufzeichnung von Mängeln am relevantesten sind. Fehler können mit hilfe der Methode in die Sammlung aufgenommen werden [`addFailure`](#método-addfailure).

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `date` | `string` | | `optional` Timetamp der Aufnahme im ISO-Format, wenn keine angegeben ist, wird die aktuelle Zeit verwendet. |
| `errors` | `{ error: string; source: string }[]` | | Fehlertabelle. Jedes Element ist ein Objekt, bei dem die Ursache des Versagens durch die Eigenschaft angegeben werden kann `source` und den entsprechenden Ausfall durch die Eigenschaft `error`. |
| `id` | `string` | | `optional` Phalle-Kennung, wenn keines angegeben ist, wird ein UUID v4 generiert. |

***

## Enum: **`SubcomponentState`**

Definiert den Zustand eines Teilbestandteils von **DriverAPI**.

| Mitglied| Wert | Beschreibung |
| :- | :- | :- |
| `running` | `0` | Unterbauteil in Betrieb und gut funktionierendem Betrieb. |
| `hold` | `1`| Als Teilkomponente wird der Betrieb eingestellt oder die Freisetzung von Ressourcen in andere Teilkomponenten erwartet. |
| `stopped` | `2` | Als Teilkomponente werden keine Operationen durchgeführt. |
| `error` | `3` | Teilkomponenten in Betrieb und mit Betriebsfehlern. |

***

## Hersteller: **`Driver`**

Erstellt eine neue Instanz der Driver-Klasse (**DriverAPI**). Es kann nur eine Instanz innerhalb eines Prozesses geben.

```typescript
Driver(options: DriverOptions): Driver
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `options` | [`DriverOptions`](#interfaz-driveroptions) | Konfigurationsoptionen für den Treiber. |

    **Rücksendungen:** `Driver`

***

## Eigentum: [**`addresses`**](./Addresses.md)

Als Adress-Repository ermöglicht dieses Objekt den Zugriff auf die Sammlung, in der die Adresseinstellungen der *Herkunft*, und ihre jeweiligen *Datenpunktsätze* y *die Daten*, die diesem Piloten gehören. Die Adresse definiert die Adresse der "realen" oder "logischen" Daten und Informationen, die der Pilot in **Netinds**.

***

## Eigentum: [**`alarms`**](./Alarms.md)

Dieses Objekt ermöglicht den Zugriff auf die Sammlung, die die vom Treiber gemeldeten Ereignisse/Alarme auf **Netinds**. Damit kann der Pilot die aktuelle Situation der veröffentlichten Alarmierungen überprüfen.

***

## Eigentum: [**`notifier`**](./Notifier.md)

Benachrichtigungsschnittstelle, dieses Objekt ermöglicht es dem Treiber, Alarmmeldungen und Ereignisse auf **Netinds**. Sie können den Zustand der Ereignisse über die Schnittstelle des Objekts überprüfen [`alarms`](#propiedad-alarmsalarmsmd).

***

## Eigentum: [**`publisher`**](./Publisher.md)

Dieses Veröffentlichungsschnittstück ermöglicht es dem Treiber, Informationen zu veröffentlichen. **Netinds** durch standardisierte Strukturen.

***

## Eigentum: **`configErrors`**

```typescript
configErrors: Multi | undefined
```

Bei Fehlern in der Konfiguration des Piloten, sowohl bei den im Hersteller angegebenen Parametern als auch in den Umgebungsvariablen, werden diese von einem Objekt des Typs angegeben. [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b).

***

## Eigentum: **`configState`**

```typescript
configState: SubcomponentState
```

Gibt den Status der Konfiguration an **DriverAPI** durch eine Variable des Typs [`SubcomponentState`](#enum-subcomponentstate);

***

## Eigentum: **`hasConfigErrors`**

```typescript
hasConfigErrors: boolean
```

Booleschen Wert, der anzeigt, ob ein Fehler in der Konfiguration vorhanden ist, sowohl bei den im Hersteller angegebenen Parametern als auch bei den Umgebungsvariablen.

***

## Eigentum: **`id`**

```typescript
id: string
```

Pilot *Kennung, die allgemein einzigartig ist* (*UUID*)

***

## Eigentum: **`isEnrolled`**

```typescript
isEnrolled: boolean
```

Boolean-Wert, der anzeigt, ob der Pilot eine Instanz von `NetinDS Agent`.

***

## Eigentum: **`state`**

```typescript
state: SubcomponentState
```

Ist der Allgemeine Zustand des **DriverAPI** durch eine Variable des Typs [`SubcomponentState`](#interfaz-subcomponentstate).

***

## Eigentum: **`status`**

```typescript
status: DriverHealthStatus
```

Indikator des *Status* des **DriverAPI** von einem Objekt des Typs [`DriverHealthStatus`](#interfaz-driverhealthstatus). Diese Art von Objekt wird in den Schnittstellen von *Beobachtbarkeit* von **Netinds**.

***

## Veranstaltung: **`done`**

Emittiert, wenn **DriverAPI** beendet eine Veröffentlichung oder Benachrichtigung.

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `jobId` | *Seil* | Arbeitskennung |
    | `result` | [`JobResult`](#interface-jobresult) | `optional` Endergebnis der Arbeit, wenn ich ohne Fehler |
    | `error` | [`Crash`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b)| `optional` Fehler, wenn Sie einen Fehler haben|

***

## Veranstaltung: **`update`**

Ausgestellt, wenn eine Version der Treiberkonfiguration verfügbar ist.

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `resourceId` | *Seil* | |-Konfigurationskennung
    | `resourceDate` | [`JobResult`](#interface-jobresult) | Datum der letzten Aktualisierung der Konfiguration |

***

## Veranstaltung: **`updated`**

Gesendet, wenn die Konfiguration des Treibers aktualisiert und verfügbar ist.

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `resourceId` | *Seil* | |-Konfigurationskennung
    | `resourceDate` | [`JobResult`](#interface-jobresult) | Datum der letzten Aktualisierung der Konfiguration |

***

## Veranstaltung: **`error`**

Emittiert, wenn ein Fehler im **DriverAPI**.

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `error` | [`Crash`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b)| Fehler im **DriverAPI** |

## Veranstaltung: **`state`**

Emittiert, wenn einer der Teilkomponenten des **DriverAPI** Ändert seinen Zustand.

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `subcomponentState` | [`SubcomponentState`](#enum-subcomponentstate)| Aktueller Stand des Bauteils |

***

## Methode: **`addFailure`**

Fügt der Bug-Sammlung ein neues Ausfallprotokoll hinzu. Diese Sammlung gehört zur Beobachtbarkeitszelle von **Netinds** und ich konnte auf der URL aufgerufen werden: `http://127.0.0.1:{PORT}/v1/registers`.

> `PORT` wird den Wert der Immobilie haben `healthPort`, dem Hersteller zugewiesen **DriverAPI**. Wenn diese Eigenschaft initialisiert wurde, wird der Wert der Umgebungsvariable sein. `CONFIG_SERVER_PORT`.

> Die Größe dieser Liste kann mit hilfe der Umgebungsvariable konfiguriert werden. `CONFIG_CONFLICT_BUFFER_LENGTH`, deren Standardwert `100`.

```typescript
addFailure(failure: DriverFailure): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `failure` | [`DriverFailure`](#interfaz-driverfailure) | Objekt, das das Scheitern der Sammlung |

    **Rücksendungen:** `void`

***

## Methode: **`update`**

Fragen Sie den **DriverAPI** die eine neue Arbeit zur Aktualisierung der Konfiguration beginnt, versucht, alle Ressourcen der Instanz des **DriverAPI**, wenn das Update erfolgreich eine Veranstaltung durchführt `updated` wird ausgestellt werden.

```typescript
update(): void
```

**Rücksendungen:** `void`

***

## Methode: **`updateSubcomponent`**

Aktualisierung oder Hinzufügen von Details des Zustands einer Unterkomponente, dies sollte verwendet werden, um zu informieren
der Zustand der Ressourcen hinter der Treiber-API, z. B. Verbindungszustände mit dem Feld
Vorrichtungen.

> Die maximale Anzahl externer Teilkomponenten beträgt 100.

```typescript
updateSubcomponent(subcomponent: SubcomponentDetail): boolean
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `subcomponent` | [`SubcomponentDetail`](#interfaz-SubcomponentDetail) | Gegenstand des Unterbaus, wird hinzugefügt oder aktualisiert, wenn es bereits |

    **Rücksendungen:** `boolean`

## Methode: **`disenroll`**

Startet den Prozess der Deabonance der Instanz des **DriverAPI** gegen den **NetinDS Agent**.

```typescript
disenroll(): Promise<void>
```

**Rücksendungen:** `Promise<void>`

***

## Methode: **`enroll`**

Startet den Abo-Prozess der Instanz des **DriverAPI** gegen den **NetinDS Agent**.

```typescript
enroll(): Promise<void>
```

**Rücksendungen:** `Promise<void>`

***
