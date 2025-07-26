# Yai Query Flow Diagram

```mermaid
flowchart TD
    A[User Types Query] --> B[UI Input Component]
    B --> C{Input Type?}
    
    C -->|Command Mode| D[UI State: Command Query]
    C -->|Chat Mode| E[UI State: Chat Query]
    
    D --> F[AI Engine - Exec Mode]
    E --> G[AI Engine - Chat Mode]
    
    F --> H[Build System Context]
    G --> H
    
    H --> I[Create OpenAI Messages]
    I --> J[OpenAI API Request]
    
    J --> K{Streaming Response}
    K --> L[Channel Stream Output]
    
    L --> M{Response Type?}
    M -->|Command| N[Extract Command]
    M -->|Chat| O[Format Chat Response]
    
    N --> P[Display Command]
    P --> Q{User Confirms?}
    Q -->|Yes| R[Command Runner]
    Q -->|No| S[Cancel Execution]
    
    R --> T[Execute Command]
    T --> U[Capture Output]
    U --> V[Display Results]
    
    O --> W[Markdown Renderer]
    W --> X[Display Chat Response]
    
    V --> Y[Update History]
    X --> Y
    S --> Y
    Y --> Z[Ready for Next Query]

    style A fill:#e1f5fe
    style Z fill:#c8e6c9
    style J fill:#fff3e0
    style R fill:#ffccbc
```

## Flow Description

1. **User Input**: User types a query in the terminal
2. **UI Processing**: Bubble Tea UI captures and processes input
3. **Mode Detection**: System determines if it's a command request or chat
4. **AI Engine**: Processes query with appropriate mode (exec/chat)
5. **Context Building**: Adds system information (OS, shell, user, etc.)
6. **OpenAI API**: Sends request with context to ChatGPT
7. **Streaming Response**: Receives and processes streaming response
8. **Response Handling**:
   - **Commands**: Extracts command, shows to user for confirmation
   - **Chat**: Renders markdown response
9. **Command Execution**: If confirmed, runs command via runner
10. **Output Display**: Shows command results or chat response
11. **History Update**: Stores interaction for future context
12. **Ready State**: Returns to input prompt for next query