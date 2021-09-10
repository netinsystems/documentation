# Mitteilung

Das Unternehmen `notifier`innerhalb von ***NetinDS DriverAPI***, ermöglicht die Meldung von Ereignissen und/oder Alarmen im Zusammenhang mit einem NetinDS-System.

Die Benachrichtigungsverfahren werden als Stellengesuche behandelt, jeder Aufruf zu einer der Methoden des `notifier` gibt einen Arbeitskünstler zurück, der verwendet werden kann, um den Abschluss des Benachrichtigungsprozesses anhand des Ereignisses zu identifizieren `done`.

Weitere Informationen über das NetinDS-Alarm- und Benachrichtigungssystem finden Sie in der [Dokumentation](https://docs.netin.io/netin_webUI/NetinDS/templates/).

## Inhaltsverzeichnis

*   [Mitteilung](#notifier)
    *   [Inhaltsverzeichnis](#tabla-de-contenidos)
    *   [Schnittstelle: **`Event`**](#interfaz-event)
    *   [Methode: **`notify` (`severity: 1`, `cot: came`)**](#método-notify-severity-1-cot-came)
    *   [Methode: **`inform` (`severity: 201`, `cot: came`)**](#método-inform-severity-201-cot-came)
    *   [Methode: **`warn` (`severity: 401`, `cot: came`)**](#método-warn-severity-401-cot-came)
    *   [Methode: **`error` (`severity: 601`, `cot: came`)**](#método-error-severity-601-cot-came)
    *   [Methode: **`critical` (`severity: 801`, `cot: came`)**](#método-critical-severity-801-cot-came)
    *   [Methode: **`deactivate` (`cot: went`)**](#método-deactivate-cot-went)

***

## Schnittstelle: **`Event`**

Die Schnittstelle `Event` legt ein Ereignis aus der Sicht des Piloten und dessen Beziehung zum NetinDS-Datenmodell fest.

| Name| Typ | Defekt | Beschreibung |
| :- | :- | :- |:- |
| `origin` | `string` | | Kennung des Geräts oder des Quellsystems des Ereignisses. <br> ***Format***: `/^[a-zA-Z0-9-_.]{1,80}$/`|
| `datapointSetId` | `string` | | Idfiator des DatapointSet, mit dem dieses Ereignis verbunden ist. <br> ***Format***: `/^[a-zA-Z0-9_-]{1,40}$/` |
| `datapointId` | `string` | | Kennung des Datenpunkts, an dem dieses Ereignis beteiligt ist. <br> ***Format***: `/^[a-zA-Z0-9#&_-]{1,80}$/` |
| `tableIndex` | `number` | `0` | `optional` Für Tabellen-Datapointsets, indexieren Sie die Tabelle, mit der dieses Ereignis verbunden ist |
| `text` | `string` | | Beschreibung des Ereignisses. <br> ***Grenze***: `240` |
| `textHelp` | `string` | | Ausführliche Informationen über das Ereignis oder wie es zu lösen ist. <br> ***Grenze***: `240` |
| `quality` | [`Quality`](https://docs.netin.io/) | `good` | `optional` Qualität des Alarms oder des Datenpunkts, auf dem, mit anderen Worten, als zuverlässig dieser Alarm oder der Wert des Datenpunkts |
| `qualityCode` | [`QualityType`](https://docs.netin.io/) | `good` | `optional` Art der Alarmqualität oder Datapoint, mit anderen Worten, der Grund für den aktuellen Wert von `quality` |
| `timestamp` | `number` | `Date.now()` | `optional` Timestamp im Epoch-Format in Millisekunden |
| `codes` | `string[]` | `[]` | `optional` Kennung des Ereignisses. Sie werden für die Differenzierung der verschiedenen Ereignisse in Fällen von Multi-Instanzen verwendet, d.h. wenn ein und derselbe `origin`, `datapointSetId` y `datapointId` verschiedene Arten von Veranstaltungen können vermögenswerte gleichzeitig |
| `facility` | `number` | `-1` | `optional` Identifizierung der Ressourcengruppe, zu der das Ereignis gehört. <br> ***Grenze***: `255` |

***

## Methode: **`notify` (`severity: 1`, `cot: came`)**

Erstellt eine Benachrichtigungs-Arbeit für ein Level-Event **Mitteilung** die an NetinDS weitergeleitet wird, wird die Arbeitsidentifikation zurückgegeben, wenn diese erstellt wird. Wenn Sie die Arbeit lösen, d.h. wenn das Ereignis an das NetinDS-System gemeldet wird, das Ereignis [`done`](./Driver.md#evento-done) mit der Arbeitskennung.

```typescript
notify(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion |

    **Rücksendungen:** `void`

```typescript
notify(event: Event): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |

    **Rücksendungen:** `Promise<string>`

***

## Methode: **`inform` (`severity: 201`, `cot: came`)**

Erstellt eine Benachrichtigungs-Arbeit für ein Level-Event **Informativ** die an NetinDS weitergeleitet wird, wird die Arbeitsidentifikation zurückgegeben, wenn diese erstellt wird. Wenn Sie die Arbeit lösen, d.h. wenn das Ereignis an das NetinDS-System gemeldet wird, das Ereignis [`done`](./Driver.md#evento-done) mit der Arbeitskennung.

```typescript
inform(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion |

    **Rücksendungen:** `void`

```typescript
inform(event: Event): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |

    **Rücksendungen:** `Promise<string>`

***

## Methode: **`warn` (`severity: 401`, `cot: came`)**

Erstellt eine Benachrichtigungs-Arbeit für ein Level-Event **Warnung** die an NetinDS weitergeleitet wird, wird die Arbeitsidentifikation zurückgegeben, wenn diese erstellt wird. Wenn Sie die Arbeit lösen, d.h. wenn das Ereignis an das NetinDS-System gemeldet wird, das Ereignis [`done`](./Driver.md#evento-done) mit der Arbeitskennung.

```typescript
warn(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion |

    **Rücksendungen:** `void`

```typescript
warn(event: Event): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |

    **Rücksendungen:** `Promise<string>`

## Methode: **`error` (`severity: 601`, `cot: came`)**

Erstellt eine Benachrichtigungs-Arbeit für ein Level-Event **Fehler** die an NetinDS weitergeleitet wird, wird die Arbeitsidentifikation zurückgegeben, wenn diese erstellt wird. Wenn Sie die Arbeit lösen, d.h. wenn das Ereignis an das NetinDS-System gemeldet wird, das Ereignis [`done`](./Driver.md#evento-done) mit der Arbeitskennung.

```typescript
error(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion |

    **Rücksendungen:** `void`

```typescript
error(event: Event): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |

    **Rücksendungen:** `Promise<string>`

***

## Methode: **`critical` (`severity: 801`, `cot: came`)**

Erstellt eine Benachrichtigungs-Arbeit für ein Level-Event **kritik** die an NetinDS weitergeleitet wird, wird die Arbeitsidentifikation zurückgegeben, wenn diese erstellt wird. Wenn Sie die Arbeit lösen, d.h. wenn das Ereignis an das NetinDS-System gemeldet wird, das Ereignis [`done`](./Driver.md#evento-done) mit der Arbeitskennung.

```typescript
critical(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion |

    **Rücksendungen:** `void`

```typescript
critical(event: Event): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung in NetinDS |

    **Rücksendungen:** `Promise<string>`

***

## Methode: **`deactivate` (`cot: went`)**

Erstellt eine Benachrichtigungsarbeit für die **Veröffentlichung** die an NetinDS weitergeleitet wird, wird die Arbeitsidentifikation zurückgegeben, wenn diese erstellt wird. Wenn Sie die Arbeit lösen, d.h. wenn das Ereignis an das NetinDS-System gemeldet wird, das Ereignis [`done`](./Driver.md#evento-done) mit der Arbeitskennung.

```typescript
deactivate(event: Event, callback: (error?: Multi, eventId?: string) => void): void
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung von NetinDS zu veröffentlichen |
    | `callback` | (`error?`: [`Multi`](https://devopmytra.visualstudio.com/NetinSystems/\_git/18796793-033e-4050-adc0-79e19a90709b), `eventId?`: *Seil*) => *Leere* | Optionale Rückruffunktion |

    **Rücksendungen:** `void`

```typescript
deactivate(event: Event): Promise<string>
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `event` | [*Veranstaltung*](#interfaz-event) | Veranstaltung von NetinDS zu veröffentlichen |

    **Rücksendungen:** `Promise<string>`
