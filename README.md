⅙- 👋 Hi, I’m @SwapTokenNFT
- 👀 I’m interested in ...
- 🌱 I’m currently learning ...
- 💞️ I’m looking to collaborate on ...
- 📫 How to reach me .[.](https://x.com/2misterX) (https://x.com/SwapTokenNFTs) (https://dorahacks.io/buidl/13438)

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
