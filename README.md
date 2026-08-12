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
flowchart TB
      subgraph User["User Request"]
          U[User clicks Sign in]
      end

      subgraph Layer1["Layer 1: Provider-Side"]
          direction TB
          P[OAuth Provider]
          P1["Internal OAuth<br/><small>Google Workspace</small>"]
          P2["Tenant Restriction<br/><small>Azure AD</small>"]
          P3["Org Membership<br/><small>GitHub</small>"]
          P --> P1 & P2 & P3
      end

      subgraph Layer2["Layer 2: Application-Side"]
          direction TB
          A[OAuth Callback]
          A1["Domain Validation<br/><small>email domain / hd claim</small>"]
          A2["Invite List<br/><small>allowed emails</small>"]
          A3["Group Membership<br/><small>provider API</small>"]
          A --> A1
          A1 --> A2
          A2 --> A3
      end

      subgraph Access["Access Granted"]
          T[Generate CKEditor Token]
          C[Real-time Collaboration]
          T --> C
      end

      U --> Layer1
      Layer1 -->|"Authenticated<br/>user profile"| Layer2
      Layer2 -->|"Validated"| Access

      Layer1 -.-|"❌ Rejected by provider"| X1[Access Denied]
      Layer2 -.-|"❌ Rejected by app"| X2[Access Denied]

      style Layer1 fill:#e3f2fd,stroke:#1976d2
      style Layer2 fill:#fff3e0,stroke:#f57c00
      style Access fill:#e8f5e9,stroke:#388e3c
      style X1 fill:#ffebee,stroke:#c62828
      style X2 fill:#ffebee,stroke:#c62828
```
