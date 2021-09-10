# Alarmanlagen

Das Unternehmen `alarms`innerhalb von ***NetinDS DriverAPI***ermöglicht den Zugriff auf das Repository für Alarme/Ereignisse, die derzeit in der DriverAPI-Instanz aktiv sind.

## Inhaltsverzeichnis

*   [Alarmanlagen](#alarms)
    *   [Inhaltsverzeichnis](#tabla-de-contenidos)
    *   [Methode: **`getAlarm`**](#método-getalarm)
    *   [Methode: **`getAlarms`**](#método-getalarms)
    *   [Methode: **`getByOrigin`**](#método-getbyorigin)
    *   [Methode: **`hasAlarm`**](#método-hasalarm)

***

## Methode: **`getAlarm`**

Gibt einen Alarm/Ereignis zurück, wenn er im Repository vorhanden ist

```typescript
getAlarm(alarmId: string): undefined | Event
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `alarmId` | *Seil* | Alarm/Ereignis |

    **Rücksendungen:** [`Event`](./Notifier.md#interfaz-event) | `undefined`

***

## Methode: **`getAlarms`**

Gibt alle im Repository vorhandenen Alarme/Ereignisse zurück

```typescript
getAlarms(): Event[][]
```

**Rücksendungen:** [`Event[][]`](./Notifier.md#interfaz-event) | `undefined`

***

## Methode: **`getByOrigin`**

Gibt alle Im Repository vorhandenen Alarme/Ereignisse für eine Quelle zurück.

```typescript
getByOrigin(origin: string): undefined | Event[]
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `origin` | *Seil* | Ursprung beantragt |

    **Rücksendungen:** [`Event[]`](./Notifier.md#interfaz-event) | `undefined`

***

## Methode: **`hasAlarm`**

Gibt ein positives Ergebnis zurück, wenn der Alarm/Ereignis im Repository vorhanden ist.

```typescript
hasAlarm(alarmId: string): boolean
```

*   **Parameter**

    | Name| Typ | Beschreibung |
    | :------ | :------ | :------ |
    | `alarmId` | *Seil* | Alarm/Ereignis |

    **Rücksendungen:** `boolean`
