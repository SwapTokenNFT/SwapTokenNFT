⅙- 👋 Hi, I’m @SwapTokenNFT
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me .[.](https://x.com/2misterX)  (https://dorahacks.io/buidl/13438)

<!---
SwapTokenNFT/SwapTokenNFT is a ✨ special ✨ repository because its `README.md` (this file) appears on your GitHub profile.
You can click the Preview link to take a look at your changes.
--->
swaptoken_nft_agent/
├── .env.example
├── .gitignore
├── README.md
├── requirements.txt
├── requirements-signer.txt
├── docker-compose.yml
├── Dockerfile.agent
├── Dockerfile.signer
├── config/
│   └── swaptoken_nft_config.json
├── output/
│   ├── audit.log
│   ├── transaction_audit.log
│   ├── state.json
│   └── report.json
├── signer_service.py
└── src/
    ├── init.py
    ├── main.py
    ├── orchestrator.py
    ├── agents.py
    ├── tools.py
    ├── wallet_guard.py
    ├── transaction_audit.py
    ├── rpc_client.py
    ├── storage.py
    ├── prompts.py
    ├── schemas.py
    └── utils.py

    flowchart LR
    U[User] --> CLI[src/main.py]
    CLI --> ORCH[src/orchestrator.py]
    ORCH --> AG[agents.py]
    AG --> WG[wallet_guard.py]
    WG --> TOOLS[tools.py]
    TOOLS --> SIGNER[signer_service.py]
    SIGNER --> AUDIT[transaction_audit.py]
    AUDIT --> STORAGE[storage.py]
    STORAGE --> OUT[output/*]

    flowchart TD
    U[Operator / User] --> CLI[src/main.py
CLI Orchestrator]

    CLI --> CMD{Command}
    CMD -->|health| H[Healthcheck]
    CMD -->|bootstrap| B[Bootstrap State]
    CMD -->|state| S[Load State]
    CMD -->|plan| P[Planning Flow]
    CMD -->|guard| G[Policy Check]
    CMD -->|simulate| SIM[Simulation Flow]
    CMD -->|submit| SUB[Submit Flow]

    subgraph Planning[Planning Layer]
        P --> PA[Product Agent]
        P --> CA[Community Agent]
        P --> COA[Content Agent]
        P --> OA[Onchain Agent]
    end

    subgraph Policy[Policy & Validation]
        G --> WG[wallet_guard.py]
        WG --> ALLOW[Allowlist Check]
        WG --> LIMITS[Amount / Daily Limits]
        WG --> APPROVAL[Human Approval Required?]
    end

    subgraph Execution[Execution Layer]
        SIM --> TOOLS[tools.py]
        SUB --> TOOLS
        TOOLS --> SIGNER[signer_service.py]
        SIGNER --> SIMRES[Simulation Result]
        SIGNER --> TXRES[Submission Result]
    end

    subgraph Audit[Audit & Storage]
        TOOLS --> TA[transaction_audit.py]
        WG --> TA
        SIGNER --> TA
        TA --> LOGS[output/transaction_audit.log]
        CLI --> ST[storage.py]
        ST --> STATE[output/state.json]
        ST --> REPORT[output/report.json]
        CLI --> ALOG[output/audit.log]
    end

    subgraph Config[Configuration]
        CFG[config/swaptoken_nft_config.json] --> CLI
        CFG --> WG
        CFG --> SIGNER
    end

    H --> HEALTH[signer /health]
    B --> STATE
    S --> STATE

    PA --> PLANOUT[Structured Plan]
    CA --> PLANOUT
    COA --> PLANOUT
    OA --> PLANOUT

    PLANOUT --> WG
    APPROVAL -->|No| SIM
    APPROVAL -->|Yes| HOLD[Pending Human Approval]
    HOLD --> SUB

    SIMRES --> SUB
    TXRES --> DONE[Done / Result to Operator]
    
## Architecture

The diagram below shows the modular agent structure of SwapToken-NFT v1.

flowchart LR
    A[SwapToken-NFT]

    subgraph M[Membership Layer]
        direction TB
        B[Club Agent]
        D[Verification Service]
        H[Admin & Moderation]
        B --> D
        B --> H
    end

    subgraph E[Economy Layer]
        direction TB
        C[Club Economy Agent]
        R[Reward Engine]
        P[Partner Hub]
        C --> R
        C --> P
    end

    subgraph G[Growth & Control]
        direction TB
        X[Analytics & Growth]
        L[Ledger & Audit]
    end

    A --> B
    A --> C
    A --> X
    A --> L

    D --> L
    R --> L
    P --> L
    H --> L
    X --> L

    subgraph BINT[Club Agent Internals]
        direction TB
        B1[Telegram]
        B2[Web App]
        B3[Partner Portal]
        B4[Membership Layer]
        B5[Community Layer]
        B6[Action Layer]
    end

    B --> B1
    B --> B2
    B --> B3
    B --> B4
    B --> B5
    B --> B6

    subgraph CINT[Club Economy Agent Internals]
        direction TB
        C1[Partner Intake]
        C2[Reward Rules]
        C3[Offer Catalog]
        C4[Economics Layer]
        C5[Partner Analytics]
    end

    C --> C1
    C --> C2
    C --> C3
    C --> C4
    C --> C5

    subgraph DINT[Verification Service Internals]
        direction TB
        D1[On-chain Verification]
        D2[Off-chain Verification]
        D3[Access Policy Engine]
        D4[Status Resolver]
    end

    D --> D1
    D --> D2
    D --> D3
    D --> D4

    subgraph RINT[Reward Engine Internals]
        direction TB
        R1[Reward Rules]
        R2[NFT Reward Minting]
        R3[Coupon / Voucher Logic]
        R4[Redemption Tracking]
    end

    R --> R1
    R --> R2
    R --> R3
    R --> R4

    subgraph PINT[Partner Hub Internals]
        direction TB
        P1[Partner Registry]
        P2[Offer Registration]
        P3[Visit Event Intake]
        P4[Partner Settlement]
    end

    P --> P1
    P --> P2
    P --> P3
    P --> P4

    subgraph XINT[Analytics & Growth Internals]
        direction TB
        X1[Retention Metrics]
        X2[LTV Metrics]
        X3[Campaign Metrics]
        X4[Growth Recommendations]
    end

    X --> X1
    X --> X2
    X --> X3
    X --> X4

    subgraph LINT[Ledger & Audit Internals]
        direction TB
        L1[Access Logs]
        L2[Reward Logs]
        L3[Partner Logs]
        L4[Campaign Logs]
        L5[Security Logs]
    end

    L --> L1
    L --> L2
    L --> L3
    L --> L4
    L --> L5

    classDef core fill:#1e3a8a,stroke:#1d4ed8,color:#ffffff,stroke-width:2px;
    classDef membership fill:#e0f2fe,stroke:#0284c7,color:#0f172a,stroke-width:1px;
    classDef economy fill:#ecfccb,stroke:#65a30d,color:#0f172a,stroke-width:1px;
    classDef growth fill:#f3e8ff,stroke:#a855f7,color:#0f172a,stroke-width:1px;
    classDef control fill:#fff7ed,stroke:#f97316,color:#0f172a,stroke-width:1px;
    classDef internals fill:#f8fafc,stroke:#94a3b8,color:#0f172a,stroke-width:1px;

    class A core;
    class B,D,H membership;
    class C,R,P economy;
    class X,L growth;
    class B1,B2,B3,B4,B5,B6,C1,C2,C3,C4,C5,D1,D2,D3,D4,R1,R2,R3,R4,P1,P2,P3,P4,X1,X2,X3,X4,L1,L2,L3,L4,L5 internals;
