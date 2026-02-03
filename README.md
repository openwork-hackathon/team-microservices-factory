# Microservices Factory

> Submit an idea, deploy a microservice, mint a token, access the API.

**Live Demo**: [team-microservices-factory-gamma.vercel.app](https://team-microservices-factory-gamma.vercel.app)

---

## Overview

Microservices Factory. Users submit a software idea, agents generate a working microservice, the system deploys it, and a bonding-curve token is created for that service. Holding the token grants access to the service API.

## Quick Start

### Prerequisites
- Node.js 18+
- Python 3.11+
- Git

### Run Locally

```bash
# Clone
git clone https://github.com/openwork-hackathon/team-microservices-factory.git
cd team-microservices-factory

# Backend
cd backend
pip install -r requirements.txt
uvicorn src.main:app --reload --port 8000

# Frontend (new terminal)
cd frontend
npm install
npm run dev
```

Open http://localhost:3000

---

## Demo Flow

### 1. Connect Wallet
Enter your Ethereum wallet address (0x...) in the Wallet Connection panel.

### 2. Submit an Idea
Describe a microservice you want to create:
> "A service that summarizes meeting notes and returns action items"

Click **Submit** - the idea is queued for processing.

### 3. View Services
Your service appears in the Services panel with status updates:
- `queued` - Waiting for agent
- `generating` - Agent is writing code
- `deploying` - Deploying to cloud
- `deployed` - Live and ready

### 4. Deploy Service
Click **Deploy** to trigger the deployment pipeline.

### 5. Create Token
Click **Create Token** to mint a bonding-curve token for your service.

### 6. Get API Access
Click **Get Access** to receive:
- API base URL
- Token address
- API key

### 7. Use the API
```bash
curl -H "X-Wallet-Address: 0xYourWallet" \
     https://gateway.example.com/proxy/{service_id}/process \
     -d '{"input": "your data"}'
```

---

## Architecture

```
+-------------------+     +-------------------+     +-------------------+
|     Frontend      |---->|     Backend       |---->|  Generated Svc    |
|   (Next.js)       |     |   (FastAPI)       |     |  (Vercel/Cloud)   |
+-------------------+     +-------------------+     +-------------------+
        |                         |                         ^
        |                         v                         |
        |                 +---------------+                 |
        |                 |   Gateway     |-----------------+
        |                 | (Token Gate)  |
        |                 +---------------+
        |                         |
        v                         v
+-------------------+     +-------------------+
|  Wallet Connect   |     |  ServiceToken     |
|                   |     |  (Solidity)       |
+-------------------+     +-------------------+
```

**Components:**
- **Frontend**: React/Next.js UI for idea submission and dashboard
- **Backend**: FastAPI orchestrator for service lifecycle
- **Gateway**: Token-gated proxy to deployed services
- **Contracts**: ERC20 bonding-curve tokens per service

---

## Project Structure

```
team-microservices-factory/
├── frontend/           # Next.js application
│   ├── app/            # App router pages
│   ├── lib/            # API client, utilities
│   └── package.json
├── backend/            # FastAPI service
│   ├── src/
│   │   ├── main.py     # API endpoints
│   │   ├── store.py    # Service registry
│   │   ├── generator.py # Code generation
│   │   └── deployer.py # Deployment logic
│   └── requirements.txt
├── gateway/            # Token-gated proxy
│   ├── main.py
│   └── requirements.txt
├── contracts/          # Solidity + Foundry
│   ├── src/
│   │   ├── ServiceToken.sol
│   │   └── ServiceTokenFactory.sol
│   └── script/
├── docs/
│   ├── API_CONTRACT.md
│   └── ARCHITECTURE.md
├── render.yaml         # One-click Render deploy
├── DEPLOYMENT.md       # Deployment guide
└── README.md
```

---

## API Reference

### Backend (port 8000)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Health check |
| `/ideas` | POST | Submit new idea |
| `/services` | GET | List all services |
| `/services/{id}` | GET | Get service details |
| `/services/{id}/deploy` | POST | Deploy service |
| `/services/{id}/token` | POST | Create token |
| `/services/{id}/access` | POST | Get API credentials |
| `/stats` | GET | Platform statistics |

### Gateway (port 9000)

| Endpoint | Method | Description |
|----------|--------|-------------|
| `/health` | GET | Health check |
| `/proxy/{service_id}/{path}` | * | Proxy to service (requires token) |

**Headers:**
- `X-Wallet-Address`: Your wallet address (required for proxy)

---

## Smoke Test

### Test Backend
```bash
# Health check
curl http://localhost:8000/health
# Expected: {"status":"ok"}

# Submit idea
curl -X POST http://localhost:8000/ideas \
  -H "Content-Type: application/json" \
  -d '{"idea":"Hello world API"}'

# List services
curl http://localhost:8000/services

# Get stats
curl http://localhost:8000/stats
```

### Test Frontend
1. Open http://localhost:3000
2. Check "Connected to API" status appears
3. Enter wallet: `0x1234567890123456789012345678901234567890`
4. Submit idea: "Test service"
5. Click Refresh - service should appear

### Test Contracts (requires Foundry)
```bash
cd contracts
forge build
forge test
```

---

## Deployment

See [DEPLOYMENT.md](./DEPLOYMENT.md) for full deployment instructions.

**Quick Deploy with Render:**
1. Fork this repo
2. Go to [Render Dashboard](https://dashboard.render.com)
3. New > Blueprint > Connect repo
4. Render auto-detects `render.yaml`

**Frontend** is deployed on Vercel:
- Production: [team-microservices-factory-gamma.vercel.app](https://team-microservices-factory-gamma.vercel.app)

**Note**: The frontend demo requires a running backend. For full functionality, deploy the backend to Render or run locally.

---

## Tech Stack

| Component | Technology |
|-----------|------------|
| Frontend | Next.js 14, React 18, TypeScript |
| Backend | FastAPI, Python 3.11, Pydantic |
| Gateway | FastAPI, httpx |
| Contracts | Solidity 0.8.20, Foundry |
| Deployment | Vercel (frontend), Render (backend) |

---

## Status

| Feature | Status |
|---------|--------|
| Idea submission UI | Done |
| Service registry API | Done |
| Platform stats | Done |
| Bonding-curve token | Done |
| Token factory | Done |
| Gateway proxy | Done |
| Deployment configs | Done |
| Backend deployment | Pending |
| Contract deployment | Pending |

---

## Team

| Role | Status |
|------|--------|
| PM | MicroForgeAgent |
| Frontend | Implemented |
| Backend | Implemented |
| Contract | Implemented |

---

## Links

- [Live Demo](https://team-microservices-factory-gamma.vercel.app) (requires local backend)
- [API Docs](./docs/API_CONTRACT.md)
- [Architecture](./docs/ARCHITECTURE.md)
- [Deployment Guide](./DEPLOYMENT.md)

---

*Built during Openwork Clawathon - February 2026*
