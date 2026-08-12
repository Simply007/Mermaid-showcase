```mermaid
sequenceDiagram
    participant B as Browser
    participant S as Server
    participant P as OAuth Provider
    participant C as CKEditor Cloud

    B->>S: Click "Sign in"
    S->>P: Redirect to OAuth consent
    P->>B: User authenticates
    P->>S: Callback with auth code
    S->>P: Exchange code for token
    P->>S: Access token + profile
    S->>B: Session cookie + redirect
    B->>S: Request CKEditor token
    S->>B: JWT token
    B->>C: Connect with JWT
    C->>B: Real-time collaboration!
```

```mermaid
flowchart TD
    subgraph Generic["Generic OAuth Flow"]
        direction TB
        subgraph Provider
            GP[OAuth Provider<br/>profile data]
        end
        subgraph Server1[Server]
            GC[OAuth Callback<br/>extract data]
            GS[Session Token<br/>store data]
            GA[Auth Status<br/>return to client]
            GT[CKEditor Token<br/>include in JWT]
        end
        subgraph CKEditor1[CKEditor]
            GE[Cloud Services<br/>presence list]
        end
        GP --> GC --> GS --> GA --> GT --> GE
    end

    subgraph Concrete["Implementation (Google)"]
        direction TB
        subgraph Google
            IG[Google Profile<br/>avatar URL]
        end
        subgraph Server2[Server]
            IP[passport.js<br/>extract ava
            IA[auth.js callback<br/>pass to session]
            IT1[tokenService<br/>session J
            IS["/auth/status<br/>return to client"]
            IT2[token.js<br/>pass to CKEdi
            IT3[tokenService<br/>CKEditor JWT]
        end
        subgraph CKEditor2[CKEditor]
            IC[Cloud Services<br/>presence
        end
        IG --> IP --> IA --> IT1 --> IS --
    end

    GP -.-> IG
    GC -.-> IP
    GS -.-> IT1
    GA -.-> IS
    GT -.-> IT2
    GE -.-> IC
```

```mermaid
flowchart TD
    subgraph Generic["Generic OAuth Flow"]
        direction TB
        subgraph Provider
            GP[OAuth Provider<br/>profile data]
        end
        subgraph Server1[Server]
            GC[OAuth Callback<br/>extract data]
            GS[Session Token<br/>store data]
            GA[Auth Status<br/>return to client]
            GT[CKEditor Token<br/>include in JWT]
        end
        subgraph CKEditor1[CKEditor]
            GE[Cloud Services<br/>presence list]
        end
        GP --> GC --> GS --> GA --> GT --> GE
    end

    subgraph Concrete["Implementation (Google)"]
        direction TB
        subgraph Google
            IG[Google Profile<br/>avatar URL]
        end
        subgraph Server2[Server]
            IP[passport.js<br/>extract avatar]
            IA[auth.js callback<br/>pass to session]
            IT1[tokenService<br/>session JWT]
            IS["/auth/status<br/>return to client"]
            IT2[token.js<br/>pass to CKEditor token]
            IT3[tokenService<br/>CKEditor JWT]
        end
        subgraph CKEditor2[CKEditor]
            IC[Cloud Services<br/>presence list]
        end
        IG --> IP --> IA --> IT1 --> IS --> IT2 --> IT3 --> IC
    end
```
