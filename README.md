- 👋 Hi, I’m @SwapTokenNFT
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
