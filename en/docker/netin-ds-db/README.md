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

## **Use**

### Default settings

The service is offered at the port `28900`.

### **Environment variables**

#### General container configuration

*   **`CONFIG_AGENT_CONFIG_FILE_PATH`** default(`undefined`): Absolute address inside the container where the configuration JSON file is located.

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

    Configuration can be used together using environment and file variables. In cases in which the same variable is configured in both ways, the configuration by file will prevail.

*   **`CONFIG_AGENT_ENTITY_UUID`** default(`'Agent'`): seed for the generation of the unique identifier of the agent. The identifier is calculated by applying according to the UUID V5 standard (RFC4122), where the seed value is used **name** and how **namespace** the following UUID `'66b9e185-a38d-4c33-ab89-5921305db9a7'`.

*   **`CONFIG_AGENT_ENTITY`** default(`'Agent'`): Legacy identification for netinDS entities.

*   **`CONFIG_AGENT_MODE`** default(undefined): The mode of operation of the agent. You can have the values `'Agent'` or `'Standalone'`. When no value is configured the system will be deployed in server mode.

*   **`CONFIG_SERVER_ADDR`** default(`'127.0.0.1'`): IP address or name of the host where the NetinDS Server associated with this agent is located. Only relevant when `CONFIG_AGENT_MODE` is configured as `'Agent'`.

*   **`CONFIG_SERVER_PORT`** default(`28800`): port where the agent management service is offered (**NetinDS-Datasource**). Only relevant when `CONFIG_AGENT_MODE` is configured as `'Agent'`.

*   **`CONFIG_ACCESS_TOKEN`** default(`' '`): Server access token. Only relevant when `CONFIG_AGENT_MODE` is configured as `'Agent'`.

*   **`CONFIG_BROKER_URL`** default(`'tcp://127.0.0.1:61616'`): Connection url to NetinDS Server data broker.Only relevant when `CONFIG_AGENT_MODE` is configured as `'Agent'`.

*   **`CONFIG_SEEDS_FOLDER`** default(`/opts/seeds`): path where the data to be pre-loaded into the system at the time of configuration is located. Within the indicated path, the system will search for the following folders: `templates`, `locations`, `addresses`, `devices` and `alarms`. The files with extension **.json** that are within these folders, will be uploaded within the corresponding collections.

## Collaboration

As in the rest of the repositories of **Netin Systems**, all collaboration is always welcome.

If you believe that an error exists, create a [bug](https://docs.microsoft.com/en-us/azure/devops/boards/backlogs/manage-bugs) providing all possible information.

If you want to make a new contribution, create a new branch, respecting at all times the flow of versions established by [GitFlow](https://www.atlassian.com/es/git/tutorials/comparing-workflows/gitflow-workflow), make your modifications to it and request approval of the same by [Pull Request](https://docs.microsoft.com/en-us/azure/devops/repos/git/pull-requests?view=azure-devops), correctly filling in the template of the same.

## License

Copyright 2021 Network Intelligence S.L. All rights reserved.

Note: All information contained herein is, and remains the property of Network Intelligence S.L. and its suppliers, if any. The intellectual and technical concepts contained herein are property of Network Intelligence S.L. and its suppliers and may be covered by European and Foreign patents, patents in process, and are protected by trade secret or copyright.
Dissemination of this information or the reproduction of this material is strictly forbidden unless prior written permission is obtained from Network Intelligence.
