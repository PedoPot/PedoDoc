# PedoConnector

## Sequence diagrams of the project workflows
### Start Listeners

```mermaid
sequenceDiagram
    activate PedoConnector
    PedoConnector->>PedoController: Ask for API credentials
    deactivate PedoConnector
    activate PedoController
    PedoController->>PedoStocker: Get API credentials
    deactivate PedoController
    activate PedoStocker
    PedoStocker-->>PedoController: Return API credentials
    deactivate PedoStocker
    activate PedoController
    PedoController-->>+PedoConnector: Send API credentials
    deactivate PedoController
    activate PedoConnector
    PedoConnector->>PedoConnector: Start listeners
    deactivate PedoConnector
```