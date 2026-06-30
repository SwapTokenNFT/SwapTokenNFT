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
    **Architecture Overview**

SwapToken-NFT is structured around a core application pipeline, a dedicated Club Agent branch for membership and community operations, and a Club Economy Agent branch for rewards and partner-driven economics. The core pipeline is responsible for orchestration, wallet protection, tool execution, transaction signing, auditing, storage, and output generation. The Club Agent uses the blockchain flow for membership verification and access management, while the Club Economy Agent uses a separate blockchain flow for rewards, redemption, and partner settlement. Shared services are reused across the system to preserve modularity, reduce duplication, and keep the architecture maintainable.
    
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

flowchart LR
    U[User]

    subgraph CORE[Core Pipeline]
        direction LR
        CLI[src/main.py]
        ORCH[src/orchestrator.py]
        AG[agents.py]
        WG[wallet_guard.py]
        TOOLS[tools.py]
        SIGNER[signer_service.py]
        AUDIT[transaction_audit.py]
        STORAGE[storage.py]
        OUT[output/*]

        CLI --> ORCH --> AG --> WG --> TOOLS --> SIGNER --> AUDIT --> STORAGE --> OUT
    end

    subgraph CLUB[Club Agent]
        direction TB
        CLUB_IN[club/intake.py]
        CLUB_VER[club/verification.py]
        CLUB_MEM[club/membership.py]
        CLUB_COMM[club/community.py]
        CLUB_ACT[club/actions.py]
        CLUB_LOG[club/club_audit.py]

        CLUB_IN --> CLUB_VER --> CLUB_MEM --> CLUB_COMM --> CLUB_ACT --> CLUB_LOG
    end

    subgraph ECON[Club Economy Agent]
        direction TB
        ECON_PARTNER[club_economy/partner_intake.py]
        ECON_RULES[club_economy/reward_rules.py]
        ECON_REW[club_economy/reward_engine.py]
        ECON_CATALOG[club_economy/offer_catalog.py]
        ECON_ANALYTICS[club_economy/partner_analytics.py]

        ECON_PARTNER --> ECON_RULES --> ECON_REW --> ECON_CATALOG --> ECON_ANALYTICS
    end

    subgraph SHARED[Shared Services]
        direction TB
        WG2[wallet_guard.py]
        TOOLS2[tools.py]
        SIGNER2[signer_service.py]
        AUDIT2[transaction_audit.py]
        STORAGE2[storage.py]
    end

    U --> CLI
    ORCH --> CLUB
    ORCH --> ECON

    CLUB_VER --> WG2
    CLUB_ACT --> TOOLS2
    CLUB_LOG --> STORAGE2

    ECON_REW --> SIGNER2
    ECON_ANALYTICS --> AUDIT2
    CLUB_LOG --> STORAGE2
    CLUB_VER --> WG2
    CLUB_ACT --> TOOLS2
    ECON_REW --> SIGNER2
    ECON_ANALYTICS --> AUDIT2

    CLI -. shared .-> WG2
    TOOLS -. shared .-> TOOLS2
    SIGNER -. shared .-> SIGNER2
    AUDIT -. shared .-> AUDIT2
    STORAGE -. shared .-> STORAGE2

    classDef core fill:#1e3a8a,stroke:#1d4ed8,color:#ffffff,stroke-width:2px;
    classDef club fill:#dbeafe,stroke:#2563eb,color:#0f172a,stroke-width:1px;
    classDef economy fill:#dcfce7,stroke:#16a34a,color:#0f172a,stroke-width:1px;
    classDef shared fill:#f3f4f6,stroke:#6b7280,color:#111827,stroke-width:1px;
    classDef storage fill:#fef3c7,stroke:#d97706,color:#111827,stroke-width:1px;

    class CLI,ORCH,AG,WG,TOOLS,SIGNER,AUDIT,STORAGE,OUT core;
    class CLUB_IN,CLUB_VER,CLUB_MEM,CLUB_COMM,CLUB_ACT,CLUB_LOG club;
    class ECON_PARTNER,ECON_RULES,ECON_REW,ECON_CATALOG,ECON_ANALYTICS economy;
    class WG2,TOOLS2,SIGNER2,AUDIT2 shared;
    class STORAGE2 storage;

######### + Blockchain 

     flowchart LR
    U[User]

    subgraph CORE[Core Pipeline]
        direction LR
        CLI[src/main.py]
        ORCH[src/orchestrator.py]
        AG[agents.py]
        WG[wallet_guard.py]
        TOOLS[tools.py]
        SIGNER[signer_service.py]
        AUDIT[transaction_audit.py]
        STORAGE[storage.py]
        OUT[output/*]

        CLI --> ORCH --> AG --> WG --> TOOLS --> SIGNER --> AUDIT --> STORAGE --> OUT
    end

    subgraph CLUB[Club Agent]
        direction TB
        CLUB_IN[club/intake.py]
        CLUB_VER[club/verification.py]
        CLUB_MEM[club/membership.py]
        CLUB_COMM[club/community.py]
        CLUB_ACT[club/actions.py]
        CLUB_LOG[club/club_audit.py]

        CLUB_IN --> CLUB_VER --> CLUB_MEM --> CLUB_COMM --> CLUB_ACT --> CLUB_LOG
    end

    subgraph ECON[Club Economy Agent]
        direction TB
        ECON_PARTNER[club_economy/partner_intake.py]
        ECON_RULES[club_economy/reward_rules.py]
        ECON_REW[club_economy/reward_engine.py]
        ECON_CATALOG[club_economy/offer_catalog.py]
        ECON_ANALYTICS[club_economy/partner_analytics.py]

        ECON_PARTNER --> ECON_RULES --> ECON_REW --> ECON_CATALOG --> ECON_ANALYTICS
    end

    subgraph CHAIN_ACCESS[Blockchain Flow: Access]
        direction TB
        A1[Build access request]
        A2[Wallet Guard]
        A3[Signer Service]
        A4[RPC / Node]
        A5[Membership Contract]
        A6[Receipt / Event Parser]

        A1 --> A2 --> A3 --> A4 --> A5 --> A6
    end

    subgraph CHAIN_REWARDS[Blockchain Flow: Rewards]
        direction TB
        R1[Build reward action]
        R2[Wallet Guard]
        R3[Signer Service]
        R4[RPC / Node]
        R5[Reward / Loyalty Contract]
        R6[Receipt / Event Parser]

        R1 --> R2 --> R3 --> R4 --> R5 --> R6
    end

    subgraph SHARED[Shared Services]
        direction TB
        WG2[wallet_guard.py]
        TOOLS2[tools.py]
        SIGNER2[signer_service.py]
        AUDIT2[transaction_audit.py]
        STORAGE2[storage.py]
    end

    U --> CLI
    ORCH --> CLUB
    ORCH --> ECON

    CLUB_VER --> A1
    CLUB_ACT --> TOOLS2
    CLUB_LOG --> STORAGE2
    A6 --> CLUB_LOG

    ECON_REW --> R1
    ECON_ANALYTICS --> AUDIT2
    R6 --> ECON_ANALYTICS

    CLI -. shared .-> WG2
    TOOLS -. shared .-> TOOLS2
    SIGNER -. shared .-> SIGNER2
    AUDIT -. shared .-> AUDIT2
    STORAGE -. shared .-> STORAGE2

    classDef core fill:#1e3a8a,stroke:#1d4ed8,color:#ffffff,stroke-width:2px;
    classDef club fill:#dbeafe,stroke:#2563eb,color:#0f172a,stroke-width:1px;
    classDef economy fill:#dcfce7,stroke:#16a34a,color:#0f172a,stroke-width:1px;
    classDef chainAccess fill:#ede9fe,stroke:#7c3aed,color:#0f172a,stroke-width:1px;
    classDef chainRewards fill:#fce7f3,stroke:#db2777,color:#0f172a,stroke-width:1px;
    classDef shared fill:#f3f4f6,stroke:#6b7280,color:#111827,stroke-width:1px;
    classDef storage fill:#fef3c7,stroke:#d97706,color:#111827,stroke-width:1px;

    class CLI,ORCH,AG,WG,TOOLS,SIGNER,AUDIT,STORAGE,OUT core;
    class CLUB_IN,CLUB_VER,CLUB_MEM,CLUB_COMM,CLUB_ACT,CLUB_LOG club;
    class ECON_PARTNER,ECON_RULES,ECON_REW,ECON_CATALOG,ECON_ANALYTICS economy;
    class A1,A2,A3,A4,A5,A6 chainAccess;
    class R1,R2,R3,R4,R5,R6 chainRewards;
    class WG2,TOOLS2,SIGNER2,AUDIT2 shared;
    class STORAGE2 storage
