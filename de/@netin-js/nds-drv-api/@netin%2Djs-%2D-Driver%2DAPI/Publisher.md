# Editor

Das Unternehmen `publisher`innerhalb von ***NetinDS DriverAPI***ermöglicht die Veröffentlichung von Informationen über ein NetinDS-System in den verschiedenen unterstützten Formaten: Zeitreihen und DatapointSet vom Typ Karte/Tabelle.

Die Veröffentlichungsprozesse werden als Stellengesuche behandelt, jeder Aufruf zu einer der Methoden des `publisher` gibt einen Arbeitskünstler zurück, der verwendet werden kann, um den Abschluss des Veröffentlichungsprozesses mit Hilfe des Ereignisses zu identifizieren. `done`.

Weitere Informationen über das NetinDS-Datenmodell finden Sie in der [Dokumentation](https://docs.netin.io/).

## Inhaltsverzeichnis

*   [Editor](#publisher)
    *   [Inhaltsverzeichnis](#tabla-de-contenidos)
    *   [Schnittstelle: **`DataSetMap`(*Karte*)**](#interfaz-datasetmapmap)
    *   [Schnittstelle: **`DataSetBulk`(*Tisch*)**](#interfaz-datasetbulktable)
    *   [Schnittstelle: **`DataSetEntry`(*Datenpunkt*)**](#interfaz-datasetentrydatapoint)
    *   [Methode: **`publish`** (*Karten*)](#método-publish-maps)
    *   [Methode: **`publishBulk`** (*Tische*)](#método-publishbulk-tables)

***

## Schnittstelle: **`DataSetMap`(*Karte*)**

Die Schnittstelle `DataSetMap` legt einen Datensatz im Kontext von NetinDS mit einem Kartenformat fest (*Schlüsselwert*).

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `origin` | `string` | | Kennung des Geräts oder des Quellsystems der Informationen. <br> ***Format***: `/^[a-zA-Z0-9-_.]{1,80}$/`|
| `datapointSetId` | `string` | | Idfiator des DatapointSet, mit dem die Informationen verknüpft sind. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/` |
| `quality` | [`Quality`](https://docs.netin.io/) | `good` | `optional` Qualität des Alarms oder des Datenpunkts, auf dem, mit anderen Worten, als zuverlässig dieser Alarm oder der Wert des Datenpunkts |
| `qualityCode` | [`QualityType`](https://docs.netin.io/) | `good` | `optional` Art der Alarmqualität oder Datapoint, mit anderen Worten, der Grund für den aktuellen Wert von `quality` |
| `timestamp` | `number` | `Date.now()` | `optional` Timestamp im Epoch-Format in Millisekunden |
| `data` | [`DataSetEntry[]`](#interfaz-datasetentry_datapoint\_) | | Im NetinDS-System zu veröffentlichenden Informationen |

***

## Schnittstelle: **`DataSetBulk`(*Tisch*)**

Die Schnittstelle `DataSetBulk` legt ein Datensatz im netinDS-Kontext mit einem Tabellenformat oder dem gleichen, einer Kartentabelle (*Schlüsselwert*).

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `origin` | `string` | | Kennung des Geräts oder des Quellsystems der Informationen. <br> ***Format***: `/^[a-zA-Z0-9-_.]{1,80}$/`|
| `datapointSetId` | `string` | | Idfiator des DatapointSet, mit dem die Informationen verknüpft sind. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/` |
| `quality` | [`Quality`](https://docs.netin.io/) | `good` | `optional` Qualität des Alarms oder des Datenpunkts, auf dem, mit anderen Worten, als zuverlässig dieser Alarm oder der Wert des Datenpunkts |
| `qualityCode` | [`QualityType`](https://docs.netin.io/) | `good` | `optional` Art der Alarmqualität oder Datapoint, mit anderen Worten, der Grund für den aktuellen Wert von `quality` |
| `timestamp` | `number` | `Date.now()` | `optional` Timestamp im Epoch-Format in Millisekunden |
| `data` | [`DataSetEntry[][]`](#interfaz-datasetentry_datapoint\_) | | Im NetinDS-System zu veröffentlichenden Informationen |

***

## Schnittstelle: **`DataSetEntry`(*Datenpunkt*)**

Die Schnittstelle `DataSetEntry` legt die Mindestinformationseinheit eines NetinDS-Systems fest, d. h. `datapoint`. Sie stellen einen realen Planwert (Messungen, Sensorzustand, Anweisungen usw.), einen logischen Zustand (logischer Zustand eines Hafens, Prozesszustand...) oder neben dem System (Meta-Informationen über Geräte, Installationen, Feldelemente...) dar.

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `datapointId` | `string` | | Identifizierer des Datenpunkts, mit dem dieser Wert verbunden ist. <br> ***Format***: `/^[a-zA-Z0-9#&_-]{1,80}$/` |
| `value` | `any` | | `optional` Behandelter und benutzerfreundlicher Wert. |
| `rawValue` | `any` | | `optional` Originalwert, wie sie aus der Quelle gesammelt wird. |
| `quality` | [`Quality`](https://docs.netin.io/) | `good` | `optional` Datenqualität, mit anderen Worten, als zuverlässig ist dieser Wert |
| `qualityCode` | [`QualityType`](https://docs.netin.io/) | `good` | `optional` Art der Datenqualität, mit anderen Worten, der Grund für den aktuellen Wert von `quality` |
| `timestamp` | `number` | `Date.now()` | `optional` Timestamp im Epoch-Format in Millisekunden |
| `meta` | `string` | | `optional` Alle Angaben über den Wert im Textformat |

> Während die Felder `value`y `rawValue`sind beide fakultativ, mindestens einer der beiden ist anzugeben.

***

## Methode: **`publish`** (*Karten*)

Erstellt eine Veröffentlichungsarbeit für ein **DataSetMaps** oder eine Reihe von **DataSetMaps** die an NetinDS weitergeleitet werden, wird die Arbeitsidentifikation zurückgegeben, wenn sie erstellt wird. Bei der Arbeitslösung, d. h. wenn die Informationen an das NetinDS-System übermittelt wurden, wird das Ereignis [`done`](./Driver.md#evento-done) mit der Arbeitskennung.

```typescript
publish(origin: string, datapointSetId: string, data: Record<string, unknown>): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `origin` | *Seil* | Kennung des Geräts oder des Quellsystems der Informationen. <br> ***Format***: `/^[a-zA-Z0-9-_.]{1,80}$/`|
    | `datapointSetId` | *Seil* | Identifikator des DatapointSet, mit dem diese Informationen verbunden sind. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/`|
    | `data` | *Dossier*\<string, unbekannt> | Im NetinDS-System zu veröffentlichenden Informationen im Format *Schlüsselwert*. Der Name jeder Eigenschaft wird als `datapointId` und der Wert dieser Eigenschaft als `rawValue` in einer Struktur des Typs [`DataEntry`](#interfaz-datasetentry_datapoint\_). |

    **Rücksendungen:** `Promise<string>`

```typescript
publish(origin: string, datapointSetId: string, data: Record<string, unknown>, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `origin` | *Seil* | Kennung des Geräts oder des Quellsystems der Informationen. |
    | `datapointSetId` | *Seil* | Identifikator des DatapointSet, mit dem diese Informationen verbunden sind. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/`|
    | `data` | *Dossier*\<string, unbekannt> | Im NetinDS-System zu veröffentlichenden Informationen im Format *Schlüsselwert*. Der Name jeder Eigenschaft wird als `datapointId` und der Wert dieser Eigenschaft als `rawValue` in einer Struktur des Typs [`DataEntry`](#interfaz-datasetentry_datapoint\_). |
    | `callback` | (`error?`: *Fehler*, `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion. |

    **Rücksendungen:** `void`

```typescript
publish(data: DataSetMap | DataSetMap[]): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `data` | [`DataSetMap`](#interfaz-datasetmap_map\_) | [`DataSetMap[]`](#interfaz-datasetmap_map\_) | Informationen, die im NetinDS-System veröffentlicht werden müssen. |

    **Rücksendungen:** `Promise<string>`

```typescript
publish(data: DataSetMap | DataSetMap[], callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `data` | [`DataSetMap`](#interfaz-datasetmap_map\_) | [`DataSetMap[]`](#interfaz-datasetmap_map\_) | Informationen, die im NetinDS-System veröffentlicht werden müssen. |
    | `callback` | (`error?`: *Fehler*, `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion. |

    **Rücksendungen:** `void`

***

## Methode: **`publishBulk`** (*Tische*)

Erstellt eine Veröffentlichungsarbeit für ein **DataSetBulk** oder eine Reihe von **DataSetBulk** die an NetinDS weitergeleitet werden, wird die Arbeitsidentifikation zurückgegeben, wenn sie erstellt wird. Bei der Arbeitslösung, d. h. wenn die Informationen an das NetinDS-System übermittelt wurden, wird das Ereignis [`done`](./Driver.md#evento-done) mit der Arbeitskennung.

```typescript
publishBulk(data: DataSetBulk | DataSetBulk[]): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `data` | [`DataSetBulk`](#interfaz-datasetbulk_table\_) | [`DataSetBulk[]`](#interfaz-datasetbulk_table\_) | Im NetinDS-System zu veröffentlichenden Informationen |

    **Rücksendungen:** `Promise<string>`

```typescript
publishBulk(data: DataSetBulk | DataSetBulk[], callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `data` | [`DataSetBulk`](#interfaz-datasetbulk_table\_) | [`DataSetBulk[]`](#interfaz-datasetbulk_table\_) | Im NetinDS-System zu veröffentlichenden Informationen |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion |

    **Rücksendungen:** `void`
