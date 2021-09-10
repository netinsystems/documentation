# **@netin-ds/netin-ds-db**

[![Known Vulnerabilities](https://img.shields.io/static/v1?style=flat\&logo=snyk\&label=Vulnerabilities\&message=0\&color=300A98F)](https://snyk.io/package/npm/snyk)
[![MongoDB](https://img.shields.io/static/v1?style=flat\&logo=MongoDB\&label=MongoDB\&message=4.4.8\&color=47a248)](https://hub.docker.com/\_/mongo?tab=description\&page=1\&ordering=last_updated)
[![Docker Image](https://img.shields.io/static/v1?style=flat\&logo=docker\&logoColor=blue\&label=dockerhub\&message=linux/amd64%20linux/arm64\&color=blue)](https://hub.docker.com/repository/docker/netinsystems/netin-ds-agent-db)

<!-- markdownlint-disable MD033 MD041 -->

<p align="center">
  <div style="text-align:center;background-color:#0091B3;">
    <img src="https://netin.io/assets/img/landing/home.svg"alt="netin"width="500">
  </div>
</p>
<!-- markdownlint-enable MD033 -->

***

## **Nutzung**

### Standardeinstellungen

Der Service wird im Hafen angeboten `28900`.

### **Umweltvariablen**

#### Allgemeiner Parameter des Behälters

*   **`CONFIG_AGENT_CONFIG_FILE_PATH`** standardmäßig,`undefined`): absolute Adresse in dem Container, in dem sich die Konfigurationsdatei befindet.

    ```json
    {
      "entityUUID": "Agent",                // Equal to CONFIG_AGENT_ENTITY_UUID
      "entity": "Agent",                    // Equal to CONFIG_AGENT_ENTITY
      "agentMode": "Agent",                 // Equal to CONFIG_AGENT_MODE
      "serverAddr": "172.16.25.26",         // Equal to CONFIG_SERVER_ADDR
      "serverPort": "28800",                // Equal to CONFIG_SERVER_PORT
      "accessToken": " ",                   // Equal to CONFIG_ACCESS_TOKEN
      "brokerURL": "tcp://127.0.0.1:61616", // Equal to CONFIG_BROKER_URL
      "seedsFolder": "/opt/seeds",          // Equal to CONFIG_SEEDS_FOLDER
    }
    ```

    Die Konfiguration kann mit Hilfe von Umgebungsvariablen und Dateien gemeinsam verwendet werden. In Fällen, in denen die gleiche Variable auf jede Weise konfiguriert ist, wird die Konfiguration pro Datei vorherrschen.

*   **`CONFIG_AGENT_ENTITY_UUID`** standardmäßig,`'Agent'`) Saatgut für die Erzeugung des einzelnen Bevollmächtigten. Der Erkennungser wird nach der Norm UUID V5 (RFC4122) berechnet, bei der der Wert des Saatguts verwendet wird **Name** und als **Raum für Namen** der folgende UUID `'66b9e185-a38d-4c33-ab89-5921305db9a7'`.

*   **`CONFIG_AGENT_ENTITY`** standardmäßig,`'Agent'`) für NetinDS-Entitäten geerbte Identifizierung.

*   **`CONFIG_AGENT_MODE`** default (undefined): Arbeitsweise des Agenten. Er kann die Werte haben. `'Agent'` o `'Standalone'`. Da kein Wert konfiguriert ist, wird das System im Server-Modus eingesetzt.

*   **`CONFIG_SERVER_ADDR`** standardmäßig,`'127.0.0.1'`) IP-Adresse oder Name des Hosts, in dem sich der mit diesem Agent verbundene NetinDS-Server befindet. Nur relevant, wenn `CONFIG_AGENT_MODE` ist so konfiguriert, dass `'Agent'`.

*   **`CONFIG_SERVER_PORT`** standardmäßig,`28800`): Hafen, in dem der Dienst für die Verwaltung der Bediensteten angeboten wird (**NetinDS-Datasource**). Nur relevant, wenn `CONFIG_AGENT_MODE` ist so konfiguriert, dass `'Agent'`.

*   **`CONFIG_ACCESS_TOKEN`** standardmäßig,`' '`) Zugriffs-Chips auf den Server. Nur relevant, wenn `CONFIG_AGENT_MODE` ist so konfiguriert, dass `'Agent'`.

*   **`CONFIG_BROKER_URL`** standardmäßig,`'tcp://127.0.0.1:61616'`) Verbindung URL mit dem NetinDS Server-Datenmakler.Nur relevant, wenn `CONFIG_AGENT_MODE` ist so konfiguriert, dass `'Agent'`.

*   **`CONFIG_SEEDS_FOLDER`** standardmäßig,`/opts/seeds`Der Pfad, auf dem sich die Daten befinden, die zum Zeitpunkt der Konfiguration im System vorverpfändet werden sollen. Auf dem angegebenen Pfad sucht das System nach folgenden Ordnern: `templates`, `locations`, `addresses`, `devices` y `alarms`. Die Dateien mit der Erweiterung **.json** die sich in diesen Ordnern befinden, werden in die entsprechenden Sammlungen geladen.

## Zusammenarbeit

Wie in anderen Benchmarks von **Netin-Systeme**, jede Zusammenarbeit wird immer gut angenommen.

Wenn Sie glauben, dass es einen Fehler gibt, erstellen Sie eine [Bug](https://docs.microsoft.com/en-us/azure/devops/boards/backlogs/manage-bugs) alle möglichen Informationen zur Verfügung stellen.

Wenn Sie einen neuen Beitrag leisten möchten, erstellen Sie einen neuen Zweig, wobei Sie jederzeit den von [Git-Flow](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow), die Änderungen vornehmen und ihre Zustimmung durch [Antrag auf Zugkraft](https://docs.microsoft.com/en-us/azure/devops/repos/git/pull-requests?view=azure-devops), indem das Modell des Modells richtig ausgefüllt wird.

## Lizenz

Copyright 2021 Network Intelligence S.L. Alle Rechte vorbehalten.

Hinweis: Alle hier enthaltenen Informationen sind und verbleiben im Eigentum von Network Intelligence S.L. und seinen Anbietern, falls vorhanden. Die hier enthaltenen intellektuellen und technischen Konzepte sind Eigentum von Network Intelligence S.L. und seinen Anbietern und können durch europäische und ausländische Patente, laufende Patente abgedeckt werden und sind durch Geschäftsgeheimnis oder Urheberrecht geschützt.
Die Verbreitung dieser Informationen oder die Vervielfältigung dieses Materials ist streng verboten, es sei denn, die vorherige schriftliche Genehmigung wird von Network Intelligence eingeholt.
