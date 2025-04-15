# PedoHunter

## Sequence diagrams of the project workflows
### Send a response to to a newly created conversation
```mermaid
sequenceDiagram
    PedoConnector->>PedoController: Received a <br/>new message
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
    PedoController->>PedoStocker: Save the response in <br/>the conversation history
    PedoController-->>PedoConnector: Send the response <br/>to the message
    deactivate PedoController
    
```
### Send a response to a conversation
```mermaid
sequenceDiagram
    PedoConnector->>PedoController: Received a <br/>new message
    activate PedoController
    PedoController->>PedoStocker: Save the new message in <br/>the conversation history
    PedoController->>+PedoHunter: Send conversation history
    deactivate PedoController
    PedoHunter-->>-PedoController: Send a response
    activate PedoController
    PedoController->>PedoStocker: Save the response in <br/>the conversation history
    PedoController-->>PedoConnector: Send the response <br/>to the message
    deactivate PedoController
```

## API usage example

```bash
curl -X POST http://localhost:8000/chat \
  -H "Content-Type: application/json" \
  -d '{
    "messages": [
      {"role": "user", "content": "Hello, i am Luca"},   
      {"role": "assistant", "content": "I am an AI assistant"},
      {"role": "user", "content": "Who am i?"}               
    ]
  }'
```
```bash
"You are Luca! 😊 \n\nIt's nice to meet you. \n\nDo you want to tell me a little bit about yourself"
```
