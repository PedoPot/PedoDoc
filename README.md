# PedoDoc

* La documentation du [PedObserver](PedObserver/README.md)
* La documentation du [PedoConnector](PedoConnector/README.md)
* La documentation du [PedoHunter](PedoHunter/README.md)
* La documentation du [PedoMeter](PedoMeter/README.md)


## Example of complete User Story
```mermaid
sequenceDiagram
    %% Manually create a new profile
    PedObserver->>+PedoController: Send new profile <br>information
    activate PedoController
    PedoController->>+PedoStocker: Add profile <br>information
    activate PedoStocker
    deactivate PedoController
    activate PedoController
    PedObserver->>+PedoController: Send new api <br>credentials
    PedoController->>+PedoStocker: Add api <br>credentials
    PedoController->>+PedoConnector: Send API credentials to start listener
    deactivate PedoController

    %% Send a response to to a newly created conversation
    PedoConnector->>PedoController: Received a new message
    activate PedoController
    PedoController->>PedoStocker: Request the conversation for<br/>the given person
    deactivate PedoController
    activate PedoStocker
    PedoStocker-->>PedoController: No person found
    deactivate PedoStocker
    activate PedoController
    PedoController->>PedoStocker: Request profile data
    deactivate PedoController
    activate PedoStocker
    PedoStocker-->>PedoController: Send the profile data
    activate PedoController
    deactivate PedoStocker
    PedoController->>PedoController: Create "system" prompt from profile data
    PedoController->>PedoStocker: Create a new conversation <br/>with as first message the prompt<br/> and the message received
    PedoController->>+PedoHunter: Send conversation
    deactivate PedoController
    PedoHunter-->>-PedoController: Send a response
    activate PedoController
    PedoController->>PedoStocker: Save the response in the conversation history
    PedoController-->>PedoConnector: Send the response <br/>to the message
    deactivate PedoController

    %% No score has been computed for the current state of the conversation
    PedObserver->>PedoController: Request a score <br>for a given suspect
    activate PedoController
    PedoController->>PedoController: No score has been saved for the current <br>conversations state of the suspect 
    PedoController->>PedoStocker: Retrieve all<br>conversations<br>for a given suspect
    deactivate PedoController
    activate PedoStocker
    PedoStocker-->>PedoController: Sends the data
    deactivate PedoStocker
    activate PedoController
    PedoController->>PedoMeter: Send all the conversations of the suspect
    deactivate PedoController
    activate PedoMeter
    PedoMeter-->>PedoController: Returns a score 
    deactivate PedoMeter
    activate PedoController
    PedoController->>PedoStocker: Saves the score<br>of the suspect for<br>the current state<br>of the conversations
    PedoController-->>PedObserver: Returns the score
    deactivate PedoController
```