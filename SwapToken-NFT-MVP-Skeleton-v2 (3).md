# SwapToken-NFT (SWNF) — MVP Skeleton

> Децентрализованный Verifiable Data Feed для ИИ на базе блокчейна SwapToken (с 2013 г.) со встроенным шифрованием.

---

## 📋 Содержание

- [Архитектура](#архитектура)
- [Быстрый старт](#быстрый-старт)
- [Структура проекта](#структура-проекта)
- [Переменные окружения](#переменные-окружения)
- [Модули](#модули)
  - [Backend (FastAPI)](#backend-fastapi)
  - [AI Agents Ecosystem](#ai-agents-ecosystem)
  - [Central Orchestrator](#central-orchestrator)
  - [Wallet Guard](#wallet-guard)
  - [Signer Service](#signer-service)
  - [TON Integration](#ton-integration)
  - [Telegram Bot](#telegram-bot)
  - [SDK](#sdk)
- [Деплой](#деплой)
- [Контакты](#контакты)

---

## 🏗 Архитектура

```
┌─────────────────────────────────────────────────────────────────┐
│                        FRONTEND LAYER                           │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ Telegram Bot │  │ Web App      │  │ SwapToken-CLUB       │  │
│  │ (Mini App)   │  │ (React/Vue)  │  │ (TMA, >10M users)    │  │
│  └──────┬───────┘  └──────┬───────┘  └──────────┬───────────┘  │
└─────────┼─────────────────┼─────────────────────┼──────────────┘
          │                 │                     │
          └─────────────────┼─────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                        API GATEWAY (FastAPI)                    │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ Auth/JWT     │  │ Rate Limit   │  │ CORS/Middleware      │  │
│  └──────┬───────┘  └──────┬───────┘  └──────────┬───────────┘  │
└─────────┼─────────────────┼─────────────────────┼──────────────┘
          │                 │                     │
          └─────────────────┼─────────────────────┘
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                    AI ORCHESTRATION LAYER                       │
│  ┌──────────────────────────────────────────────────────────┐  │
│  │              CENTRAL ORCHESTRATOR                        │  │
│  │  ┌─────────────┐ ┌─────────────┐ ┌────────────────────┐ │  │
│  │  │ Task Router │ │ Scheduler   │ │ Health Monitor     │ │  │
│  │  └──────┬──────┘ └──────┬──────┘ └─────────┬──────────┘ │  │
│  └─────────┼───────────────┼──────────────────┼────────────┘  │
            │               │                  │
            ▼               ▼                  ▼
  ┌──────────────────────────────────────────────────────────┐
  │              CORE AI AGENTS (8)                          │
  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────┐ │
  │  │ AI       │ │ Strategic│ │ Marketing│ │ Customer     │ │
  │  │Supervisor│ │ Growth   │ │ Agent    │ │ Success      │ │
  │  └──────────┘ └──────────┘ └──────────┘ └──────────────┘ │
  │  ┌──────────┐ ┌──────────┐ ┌──────────┐ ┌──────────────┐ │
  │  │ Club     │ │ Club     │ │ Wallet   │ │ Data Feed    │ │
  │  │ Agent    │ │ Economy  │ │ Guard    │ │ / Security   │ │
  │  └──────────┘ └──────────┘ └──────────┘ └──────────────┘ │
  └──────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      SECURITY LAYER                             │
│  ┌──────────────────┐  ┌─────────────────────────────────────┐  │
│  │   WALLET GUARD   │  │         SIGNER SERVICE              │  │
│  │  ┌────────────┐  │  │  ┌────────────┐  ┌──────────────┐  │  │
│  │  │ Validation │  │  │  │ HSM/TEE    │  │ Multi-sig    │  │  │
│  │  │ Whitelist  │  │  │  │ Key Mgmt   │  │ Threshold    │  │  │
│  │  │ Rate Limit │  │  │  │ Signing    │  │ Recovery     │  │  │
│  │  └────────────┘  │  │  └────────────┘  └──────────────┘  │  │
│  └──────────────────┘  └─────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      BLOCKCHAIN LAYER                           │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │                    TON BLOCKCHAIN                         │  │
│  │  ┌──────────────┐  ┌──────────────┐  ┌───────────────┐   │  │
│  │  │ NFT Contract │  │ Token        │  │ Data Oracle   │   │  │
│  │  │ (FunC)       │  │ Contract     │  │ Contract      │   │  │
│  │  └──────────────┘  └──────────────┘  └───────────────┘   │  │
│  │                                                           │  │
│  │  Contract: EQCnuAkqB_ZC8fji5JNJGh9WbsVrCprOmgKz1j60aGKHTE63│  │
│  └───────────────────────────────────────────────────────────┘  │
│                                                                 │
│  ┌───────────────────────────────────────────────────────────┐  │
│  │              SWAPTOKEN BLOCKCHAIN (Private)               │  │
│  │         (Satoshi-based, since 2013)                       │  │
│  └───────────────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
                            │
                            ▼
┌─────────────────────────────────────────────────────────────────┐
│                      DATA LAYER                                 │
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────────────┐  │
│  │ PostgreSQL   │  │ Redis        │  │ IPFS/Filecoin        │  │
│  │ (Main DB)    │  │ (Cache/Queue)│  │ (Metadata Storage)   │  │
│  └──────────────┘  └──────────────┘  └──────────────────────┘  │
└─────────────────────────────────────────────────────────────────┘
```

---

## 🚀 Быстрый старт

### Требования

- Python 3.11+
- PostgreSQL 14+
- Redis 7+
- Node.js 18+ (для TON SDK)
- Docker & Docker Compose (опционально)

### Установка

```bash
# 1. Клонирование репозитория
git clone https://github.com/SwapTokenNFT/SwapToken-NFT.git
cd SwapToken-NFT

# 2. Создание виртуального окружения
python -m venv venv
source venv/bin/activate  # Linux/Mac
# venv\Scripts\activate  # Windows

# 3. Установка зависимостей
pip install -r requirements.txt

# 4. Настройка переменных окружения
cp .env.example .env
# Отредактируйте .env файл

# 5. Инициализация базы данных
alembic upgrade head

# 6. Запуск
uvicorn backend.main:app --reload --host 0.0.0.0 --port 8000
```

### Docker (рекомендуется для продакшена)

```bash
# Сборка и запуск всех сервисов
docker-compose up --build -d

# Проверка статуса
docker-compose ps

# Логи
docker-compose logs -f backend
```

---

## 📁 Структура проекта

```
swaptoken-nft-mvp-skeleton/
│
├── backend/
│   ├── main.py                    # Точка входа FastAPI
│   ├── config.py                  # Конфигурация
│   ├── api/
│   │   ├── v1/
│   │   │   ├── endpoints/
│   │   │   │   ├── auth.py
│   │   │   │   ├── nfts.py
│   │   │   │   ├── data_feeds.py
│   │   │   │   ├── trading.py
│   │   │   │   └── agents.py
│   │   │   └── router.py
│   │   └── deps.py
│   ├── core/
│   │   ├── security.py
│   │   ├── exceptions.py
│   │   └── middleware.py
│   ├── models/
│   │   ├── base.py
│   │   ├── user.py
│   │   ├── nft.py
│   │   ├── data_feed.py
│   │   └── transaction.py
│   ├── schemas/
│   │   ├── user.py
│   │   ├── nft.py
│   │   └── data_feed.py
│   ├── services/
│   │   ├── blockchain.py
│   │   ├── data_verification.py
│   │   └── encryption.py
│   ├── agents/
│   │   ├── orchestrator.py        # Central Orchestrator
│   │   ├── supervisor.py          # AI Supervisor
│   │   ├── marketing.py           # Marketing Agent
│   │   ├── customer_success.py    # Customer Success Agent
│   │   ├── club_agent.py          # Club Agent
│   │   ├── club_economy.py        # Club Economy
│   │   ├── wallet_guard.py        # Wallet Guard Agent
│   │   └── strategies/
│   │       ├── data_feed.py
│   │       ├── security.py
│   │       ├── trading.py
│   │       └── growth.py
│   └── security/
│       ├── wallet_guard.py        # Wallet Guard Core
│       └── signer.py              # Signer Service
│
├── contracts/
│   ├── swaptoken-nft-contract.fc  # TON NFT Contract (FunC)
│   ├── token-contract.fc
│   └── oracle-contract.fc
│
├── telegram_bot/
│   ├── bot.py
│   ├── handlers/
│   │   ├── start.py
│   │   ├── nfts.py
│   │   ├── wallet.py
│   │   └── admin.py
│   └── keyboards/
│       └── main.py
│
├── sdk/
│   ├── python/
│   │   └── swaptoken_sdk/
│   │       ├── client.py
│   │       ├── nft.py
│   │       └── data_feed.py
│   └── javascript/
│       └── swaptoken-sdk/
│           ├── src/
│           │   ├── client.ts
│           │   ├── nft.ts
│           │   └── dataFeed.ts
│           └── package.json
│
├── migrations/                    # Alembic миграции
├── tests/
│   ├── unit/
│   ├── integration/
│   └── e2e/
│
├── docker/
│   ├── Dockerfile.backend
│   ├── Dockerfile.bot
│   └── docker-compose.yml
│
├── docs/
│   ├── API.md
│   ├── ARCHITECTURE.md
│   └── DEPLOYMENT.md
│
├── .env.example
├── requirements.txt
├── pyproject.toml
└── README.md
```

---

## 🔐 Переменные окружения

Создайте файл `.env` на основе `.env.example`:

```env
# === APP ===
APP_NAME=SwapToken-NFT
APP_ENV=development
DEBUG=true
SECRET_KEY=your-super-secret-key-here

# === DATABASE ===
DATABASE_URL=postgresql://user:password@localhost:5432/swaptoken_nft
DATABASE_POOL_SIZE=20

# === REDIS ===
REDIS_URL=redis://localhost:6379/0
REDIS_PASSWORD=

# === TON BLOCKCHAIN ===
TON_API_KEY=your-toncenter-api-key
TON_CONTRACT_ADDRESS=EQCnuAkqB_ZC8fji5JNJGh9WbsVrCprOmgKz1j60aGKHTE63
TON_WALLET_MNEMONIC=word1 word2 word3 ... word24
TON_NETWORK=mainnet

# === SWAPTOKEN BLOCKCHAIN ===
SWAPTOKEN_RPC_URL=https://rpc.swaptoken.io
SWAPTOKEN_CHAIN_ID=1

# === SECURITY ===
WALLET_GUARD_ENABLED=true
WALLET_GUARD_MAX_TX_PER_MIN=10
SIGNER_HSM_ENABLED=false
SIGNER_TEE_ENABLED=true
MULTISIG_THRESHOLD=2

# === AI / ORCHESTRATOR ===
ORCHESTRATOR_MAX_AGENTS=10
ORCHESTRATOR_TASK_TIMEOUT=300
SUPERVISOR_MODEL=gpt-4

# === TELEGRAM ===
TELEGRAM_BOT_TOKEN=your-bot-token
TELEGRAM_WEBHOOK_URL=https://your-domain.com/webhook
TELEGRAM_MINI_APP_URL=https://t.me/NFTswaptoken_bot

# === IPFS ===
IPFS_GATEWAY=https://ipfs.io
IPFS_PINNING_SERVICE=pinata
PINATA_API_KEY=
PINATA_SECRET_KEY=

# === MONITORING ===
SENTRY_DSN=
PROMETHEUS_PORT=9090
GRAFANA_PORT=3000
```

---

## 🧩 Модули

### Backend (FastAPI)

Основной API-сервер на FastAPI:

```python
# backend/main.py
from fastapi import FastAPI
from backend.api.v1.router import api_router

app = FastAPI(
    title="SwapToken-NFT API",
    version="1.0.0",
    docs_url="/docs",
    redoc_url="/redoc"
)

app.include_router(api_router, prefix="/api/v1")
```

**Основные эндпоинты:**

| Метод | Путь | Описание |
|-------|------|----------|
| POST | `/api/v1/auth/login` | Авторизация |
| POST | `/api/v1/auth/register` | Регистрация |
| GET | `/api/v1/nfts` | Список NFT |
| GET | `/api/v1/nfts/{id}` | Детали NFT |
| POST | `/api/v1/nfts/mint` | Минтинг NFT |
| GET | `/api/v1/data-feeds` | Data Feeds |
| POST | `/api/v1/data-feeds/verify` | Верификация данных |
| GET | `/api/v1/agents/status` | Статус агентов |

---

### AI Agents Ecosystem

Полная агентная экосистема из **8 специализированных агентов**, управляемых Central Orchestrator:

```python
# backend/agents/ecosystem.py
class AIAgentEcosystem:
    def __init__(self):
        self.agents = {
            # Core AI Agents (8)
            'orchestrator': CentralOrchestrator(),
            'ai_supervisor': AISupervisor(),
            'strategic_growth': StrategicGrowthAgent(),
            'marketing': MarketingAgent(),
            'customer_success': CustomerSuccessAgent(),
            'club_agent': ClubAgent(),
            'club_economy': ClubEconomyAgent(),
            'wallet_guard': WalletGuardAgent(),
            # Domain Agents (под надзором AI Supervisor)
            'data_feed': DataFeedAgent(),
            'security': SecurityAgent(),
            'trading': TradingAgent()
        }

    async def execute_task(self, agent_type: str, task: dict):
        agent = self.agents.get(agent_type)
        if not agent:
            raise ValueError(f"Unknown agent type: {agent_type}")
        return await agent.process(task)
```

**Core AI Agents (8):**

| # | Агент | Функция | Модель | Приоритет |
|---|-------|---------|--------|-----------|
| 1 | **Orchestrator** | Центральное управление задачами и маршрутизация | Custom | P0 |
| 2 | **AI Supervisor** | Надзор, аудит и коррекция поведения агентов | GPT-4 | P0 |
| 3 | **Marketing Agent** | Маркетинг, кампании, аналитика охвата | GPT-4 | P1 |
| 4 | **Customer Success** | Поддержка пользователей, onboarding, retention | GPT-4 | P1 |
| 5 | **Strategic Growth** | Рост экосистемы, партнёрства, BD | GPT-4 | P2 |
| 6 | **Club Agent** | Управление SwapToken-CLUB, тиры, доступ | GPT-4 | P2 |
| 7 | **Club Economy** | Экономика клуба, токеномика, rewards | GPT-4 + фин. модели | P2 |
| 8 | **Wallet Guard** | Безопасность кошельков, аномалии, защита | GPT-4 + кастомные правила | P0 |

**Domain Agents (под надзором AI Supervisor):**

| Агент | Функция | Модель |
|-------|---------|--------|
| Data Feed Agent | Верификация и агрегация данных | GPT-4 |
| Security Agent | Мониторинг угроз и аномалий | GPT-4 + кастомные правила |
| Trading Agent | Анализ рынка и торговые сигналы | GPT-4 + фин. модели |

**Приоритет внедрения:** `Marketing → Customer Success → Analytics → Support → Compliance`

---

### Central Orchestrator

Центральный оркестратор задач между агентами:

```python
# backend/agents/orchestrator.py
class CentralOrchestrator:
    def __init__(self):
        self.task_queue = asyncio.Queue()
        self.agents_pool = AgentPool(max_size=10)
        self.scheduler = TaskScheduler()

    async def submit_task(self, task: Task) -> TaskResult:
        # Маршрутизация задачи к подходящему агенту
        agent = await self.route_task(task)
        result = await agent.execute(task)
        await self.verify_result(result)
        return result

    async def route_task(self, task: Task) -> Agent:
        # Интеллектуальная маршрутизация
        if task.type == 'data_verification':
            return self.agents_pool.get('data_feed')
        elif task.type == 'security_check':
            return self.agents_pool.get('security')
        elif task.type == 'marketing_campaign':
            return self.agents_pool.get('marketing')
        elif task.type == 'customer_support':
            return self.agents_pool.get('customer_success')
        elif task.type == 'club_management':
            return self.agents_pool.get('club_agent')
        elif task.type == 'economy_analysis':
            return self.agents_pool.get('club_economy')
        # ...
```

**Возможности:**
- Приоритизация задач
- Балансировка нагрузки между агентами
- Мониторинг health-check
- Автоматический retry при ошибках
- Сбор метрик производительности

---

### AI Supervisor

Агент надзора за всей агентной экосистемой:

```python
# backend/agents/supervisor.py
class AISupervisor:
    def __init__(self):
        self.agents = {
            'data_feed': DataFeedAgent(),
            'security': SecurityAgent(),
            'trading': TradingAgent(),
            'growth': StrategicGrowthAgent(),
            'marketing': MarketingAgent(),
            'customer_success': CustomerSuccessAgent(),
            'club_agent': ClubAgent(),
            'club_economy': ClubEconomyAgent()
        }
        self.audit_log = []

    async def supervise(self, agent_type: str, task: dict, result: dict):
        # Аудит результата
        audit = await self.audit_task(agent_type, task, result)
        # Коррекция при отклонениях
        if not audit.is_valid:
            await self.correct_agent(agent_type, audit)
        self.audit_log.append(audit)
        return audit
```

---

### Marketing Agent

```python
# backend/agents/marketing.py
class MarketingAgent:
    async def run_campaign(self, campaign: Campaign):
        # Анализ целевой аудитории
        audience = await self.analyze_audience(campaign.target)
        # Генерация контента
        content = await self.generate_content(campaign.theme)
        # Запуск на каналах
        await self.distribute(content, channels=['telegram', 'x', 'discord'])
        # Сбор метрик
        return await self.collect_metrics(campaign.id)
```

---

### Customer Success Agent

```python
# backend/agents/customer_success.py
class CustomerSuccessAgent:
    async def handle_user(self, user_id: str, intent: str):
        # Определение контекста
        context = await self.get_user_context(user_id)
        # Генерация ответа
        response = await self.generate_response(intent, context)
        # Эскалация при необходимости
        if response.confidence < 0.7:
            await self.escalate_to_human(user_id, intent)
        return response
```

---

### Club Agent

```python
# backend/agents/club_agent.py
class ClubAgent:
    async def manage_tier(self, user_id: str, action: str):
        # Проверка текущего тира
        current_tier = await self.get_user_tier(user_id)
        # Обновление привилегий
        if action == 'upgrade':
            await self.upgrade_tier(user_id)
        elif action == 'downgrade':
            await self.downgrade_tier(user_id)
        # Синхронизация с NFT
        await self.sync_nft_tier(user_id)
```

---

### Club Economy

```python
# backend/agents/club_economy.py
class ClubEconomyAgent:
    async def calculate_rewards(self, user_id: str):
        # Анализ активности
        activity = await self.get_activity_score(user_id)
        # Расчёт rewards
        rewards = self.reward_formula(activity)
        # Распределение токенов
        await self.distribute_rewards(user_id, rewards)
        return rewards
```

---

### Wallet Guard

Модуль безопасности кошельков (агент + core):

```python
# backend/security/wallet_guard.py
class WalletGuard:
    def __init__(self):
        self.whitelist = set()
        self.blacklist = set()
        self.rate_limiter = RateLimiter()

    async def validate_transaction(self, tx: Transaction) -> bool:
        # Проверка адреса
        if tx.to_address in self.blacklist:
            raise SecurityError("Address blacklisted")

        # Rate limiting
        if not await self.rate_limiter.check(tx.from_address):
            raise RateLimitError("Too many transactions")

        # Проверка суммы
        if tx.amount > self.max_tx_amount:
            raise SecurityError("Amount exceeds limit")

        return True
```

**Функции:**
- Валидация адресов (whitelist/blacklist)
- Rate limiting (транзакции/минута)
- Проверка лимитов сумм
- Анализ паттернов (аномалии)
- Интеграция с AI Security Agent

---

### Signer Service

Сервис подписи транзакций с HSM/TEE:

```python
# backend/security/signer.py
class SignerService:
    def __init__(self):
        self.hsm = HSMProvider() if config.HSM_ENABLED else None
        self.tee = TEEProvider() if config.TEE_ENABLED else None
        self.multisig = MultiSigWallet(threshold=config.MULTISIG_THRESHOLD)

    async def sign_transaction(self, tx: Transaction) -> SignedTx:
        # Генерация подписи в защищённом окружении
        if self.tee:
            signature = await self.tee.sign(tx.hash)
        elif self.hsm:
            signature = await self.hsm.sign(tx.hash)
        else:
            signature = await self._software_sign(tx.hash)

        # Multi-sig если требуется
        if self.multisig.is_enabled():
            signature = await self.multisig.co_sign(signature)

        return SignedTx(tx=tx, signature=signature)
```

**Уровни безопасности:**
1. **TEE (Trusted Execution Environment)** — Intel SGX/AMD SEV
2. **HSM (Hardware Security Module)** — YubiHSM, AWS CloudHSM
3. **Software** — зашифрованное хранилище (fallback)

---

### TON Integration

Интеграция с TON блокчейном через FunC контракты:

```func
;; contracts/swaptoken-nft-contract.fc
;; NFT Item Contract

() recv_internal(int msg_value, cell in_msg_full, slice in_msg_body) impure {
    ;; Обработка сообщений
    int op = in_msg_body~load_uint(32);

    if (op == op::transfer) {
        ;; Логика трансфера NFT
        transfer_nft(in_msg_body);
    }
    elseif (op == op::mint) {
        ;; Минтинг нового NFT
        mint_nft(in_msg_body);
    }
    ;; ...
}
```

**Контракты:**
- **NFT Contract** — стандарт TEP-62 с кастомными расширениями
- **Token Contract** — Jetton-совместимый токен
- **Oracle Contract** — верификация данных для AI

**Адрес контракта:** `EQCnuAkqB_ZC8fji5JNJGh9WbsVrCprOmgKz1j60aGKHTE63`

---

### Telegram Bot

Telegram Mini App + Bot для взаимодействия с экосистемой:

```python
# telegram_bot/bot.py
from aiogram import Bot, Dispatcher

bot = Bot(token=config.TELEGRAM_BOT_TOKEN)
dp = Dispatcher()

@dp.message(Command("start"))
async def cmd_start(message: Message):
    await message.answer(
        "👋 Добро пожаловать в SwapToken-NFT!\n\n"
        "Выберите действие:",
        reply_markup=main_keyboard()
    )

# Запуск Mini App
@dp.message(Command("app"))
async def cmd_app(message: Message):
    await message.answer(
        "🚀 Откройте Mini App:",
        reply_markup=InlineKeyboardMarkup(
            inline_keyboard=[
                [InlineKeyboardButton(
                    text="Открыть SwapToken-CLUB",
                    web_app=WebAppInfo(url=config.MINI_APP_URL)
                )]
            ]
        )
    )
```

**Команды бота:**

| Команда | Описание |
|---------|----------|
| `/start` | Начало работы |
| `/wallet` | Управление кошельком |
| `/nfts` | Мои NFT |
| `/market` | NFT-маркетплейс |
| `/app` | Открыть Mini App |
| `/admin` | Панель администратора |

**NFT Тиры (SwapToken-CLUB):**
- 🥉 Guest — базовый доступ
- 🥈 Member — расширенные функции
- 🥇 Premium — полный доступ + привилегии
- 👑 Admin — управление

---

### SDK

Клиентские библиотеки для интеграции:

**Python SDK:**
```python
from swaptoken_sdk import SwapTokenClient

client = SwapTokenClient(api_key="your-api-key")

# Получить NFT
nft = await client.nft.get("EQ...")

# Минтинг
result = await client.nft.mint(
    owner_address="EQ...",
    metadata={"name": "My NFT", "image": "ipfs://..."}
)

# Data Feed
feed = await client.data_feed.subscribe("price_eth_usd")
```

**JavaScript/TypeScript SDK:**
```typescript
import { SwapTokenClient } from '@swaptoken/sdk';

const client = new SwapTokenClient({ apiKey: 'your-api-key' });

// Подключение кошелька
await client.wallet.connect('tonkeeper');

// Покупка NFT
const tx = await client.nft.buy('EQ...');
await tx.wait();
```

---

## 🚢 Деплой

### Production Docker Compose

```yaml
# docker/docker-compose.yml
version: '3.8'

services:
  backend:
    build:
      context: ..
      dockerfile: docker/Dockerfile.backend
    ports:
      - "8000:8000"
    environment:
      - DATABASE_URL=postgresql://postgres:postgres@db:5432/swaptoken_nft
      - REDIS_URL=redis://redis:6379/0
    depends_on:
      - db
      - redis
    restart: unless-stopped

  telegram_bot:
    build:
      context: ..
      dockerfile: docker/Dockerfile.bot
    environment:
      - TELEGRAM_BOT_TOKEN=${TELEGRAM_BOT_TOKEN}
    depends_on:
      - backend
    restart: unless-stopped

  db:
    image: postgres:15-alpine
    volumes:
      - postgres_data:/var/lib/postgresql/data
    environment:
      - POSTGRES_USER=postgres
      - POSTGRES_PASSWORD=postgres
      - POSTGRES_DB=swaptoken_nft
    restart: unless-stopped

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data
    restart: unless-stopped

  nginx:
    image: nginx:alpine
    ports:
      - "80:80"
      - "443:443"
    volumes:
      - ./nginx.conf:/etc/nginx/nginx.conf
      - ./ssl:/etc/nginx/ssl
    depends_on:
      - backend
    restart: unless-stopped

volumes:
  postgres_data:
  redis_data:
```

### Деплой на сервер

```bash
# 1. Клонирование на сервер
git clone https://github.com/SwapTokenNFT/SwapToken-NFT.git
cd SwapToken-NFT

# 2. Настройка окружения
cp .env.example .env
nano .env  # Редактирование

# 3. Запуск
docker-compose -f docker/docker-compose.yml up -d

# 4. Миграции
docker-compose exec backend alembic upgrade head

# 5. Проверка
curl http://localhost:8000/health
```

---

## 📊 Мониторинг

### Prometheus метрики

```python
# Доступны по адресу /metrics
from prometheus_client import Counter, Histogram

tx_counter = Counter('swaptoken_transactions_total', 'Total transactions')
tx_duration = Histogram('swaptoken_transaction_duration_seconds', 'Transaction duration')
```

### Grafana дашборды

- Статус агентов
- Транзакции в реальном времени
- Health checks
- AI model performance

---

## 🧪 Тестирование

```bash
# Unit тесты
pytest tests/unit -v

# Интеграционные тесты
pytest tests/integration -v

# E2E тесты
pytest tests/e2e -v

# Покрытие
pytest --cov=backend --cov-report=html
```

---

## 📚 Дополнительная документация

- [API Documentation](./docs/API.md) — полная документация REST API
- [Architecture Deep Dive](./docs/ARCHITECTURE.md) — подробная архитектура
- [Deployment Guide](./docs/DEPLOYMENT.md) — руководство по деплою

---

## 🔗 Ссылки

| Ресурс | Ссылка |
|--------|--------|
| DoraHacks | [dorahacks.io/buidl/13438](https://dorahacks.io/buidl/13438) |
| X (Twitter) | [@SwapTokenNFTs](https://x.com/SwapTokenNFTs) |
| GitHub | [SwapTokenNFT](https://github.com/SwapTokenNFT) |
| Telegram Bot | [@NFTswaptoken_bot](https://t.me/NFTswaptoken_bot) |
| GetGems | [getgems.io](https://getgems.io) |
| Blum Memepad | SWNF_P13io |

---

## 📄 Лицензия

MIT License — см. [LICENSE](./LICENSE)

---

## 🤝 Контрибуция

Мы приветствуем вклад в проект! См. [CONTRIBUTING.md](./CONTRIBUTING.md)

---

<p align="center">
  <strong>SwapToken-NFT (SWNF)</strong><br>
  Децентрализованный Verifiable Data Feed для ИИ на базе блокчейна SwapToken
</p>
