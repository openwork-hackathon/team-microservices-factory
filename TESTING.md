# Testing Guide

Instructions for testing the Microservices Factory locally and in production.

---

## Quick Smoke Test

### 1. Backend Health Check

```bash
curl http://localhost:8000/health
```

Expected:
```json
{"status":"ok"}
```

### 2. Frontend Load Test

Open http://localhost:3000 and verify:
- [ ] Page loads without errors
- [ ] "Microservices Factory" title visible
- [ ] Wallet Connection panel visible
- [ ] Submit Idea panel visible
- [ ] Services panel visible

### 3. API Status Check

On the frontend, verify:
- [ ] "Connected to API" message appears (green)
- OR
- [ ] "Backend API unavailable" message with retry button (red)

---

## Full Test Suite

### Backend Tests

```bash
cd backend

# Install test dependencies
pip install pytest httpx

# Run tests
pytest -v
```

Test coverage:
- Health endpoint
- Idea submission
- Service listing
- Service retrieval
- Deploy endpoint
- Token creation
- Access generation
- Stats endpoint

### Contract Tests

```bash
cd contracts

# Install Foundry if needed
curl -L https://foundry.paradigm.xyz | bash
foundryup

# Run tests
forge test -vv
```

Test coverage:
- ServiceToken deployment
- Buy tokens (bonding curve)
- Sell tokens (bonding curve)
- Price calculation
- Transfer tokens
- ServiceTokenFactory deployment
- Create multiple tokens

### Frontend Tests

```bash
cd frontend

# Type checking
npx tsc --noEmit

# Lint
npm run lint
```

---

## Manual Test Checklist

### Wallet Connection
- [ ] Enter valid address (0x + 40 hex chars) - shows "Wallet connected"
- [ ] Enter invalid address - shows error message
- [ ] Clear address - removes connected status

### Idea Submission
- [ ] Submit empty idea - button disabled
- [ ] Submit valid idea - creates service, clears input
- [ ] Service appears in list after submission
- [ ] Stats update after submission

### Service Operations
- [ ] Click Refresh - reloads service list
- [ ] Click Deploy on queued service - status updates
- [ ] Click Create Token on deployed service - token address appears
- [ ] Click Get Access on tokenized service - shows credentials
- [ ] Click Load Events - shows event history

### Error Handling
- [ ] Stop backend - frontend shows "Backend API unavailable"
- [ ] Click Retry Connection - attempts reconnect
- [ ] Submit idea without backend - shows network error

---

## API Test Examples

### Submit Idea

```bash
curl -X POST http://localhost:8000/ideas \
  -H "Content-Type: application/json" \
  -d '{
    "idea": "A service that converts markdown to HTML",
    "requester_id": "test-user-123"
  }'
```

Response:
```json
{
  "id": "abc123...",
  "idea": "A service that converts markdown to HTML",
  "requester_id": "test-user-123",
  "status": "queued",
  "token_address": null,
  "api_base_url": null,
  "created_at": "2026-02-03T...",
  "updated_at": "2026-02-03T..."
}
```

### List Services

```bash
curl http://localhost:8000/services
```

### Get Service

```bash
curl http://localhost:8000/services/{service_id}
```

### Deploy Service

```bash
curl -X POST http://localhost:8000/services/{service_id}/deploy
```

### Create Token

```bash
curl -X POST http://localhost:8000/services/{service_id}/token
```

### Get Access

```bash
curl -X POST http://localhost:8000/services/{service_id}/access
```

### Platform Stats

```bash
curl http://localhost:8000/stats
```

Response:
```json
{
  "total_services": 5,
  "status_counts": {
    "queued": 2,
    "deployed": 3
  },
  "deployed_count": 3,
  "tokenized_count": 2
}
```

---

## Gateway Test

### Without Token (Should Fail)

```bash
curl http://localhost:9000/proxy/service123/health
```

Response: `403 Forbidden - Token access required`

### With Dev Bypass

```bash
curl -H "X-Dev-Bypass: 1" \
     http://localhost:9000/proxy/service123/health
```

### With Wallet Address

```bash
curl -H "X-Wallet-Address: 0x742d35Cc6634C0532925a3b844Bc9e7595f2bD10" \
     http://localhost:9000/proxy/service123/health
```

---

## Contract Test Commands

### Build

```bash
forge build
```

### Test

```bash
forge test
```

### Test with Verbosity

```bash
forge test -vvv
```

### Test Specific Contract

```bash
forge test --match-contract ServiceTokenTest
```

### Gas Report

```bash
forge test --gas-report
```

---

## Production Smoke Test

### Frontend (Vercel)

```bash
curl -s https://team-microservices-factory-gamma.vercel.app | head -20
```

Verify: HTML response with "Microservices Factory"

### Backend (when deployed)

```bash
curl https://msf-backend.onrender.com/health
```

### Gateway (when deployed)

```bash
curl https://msf-gateway.onrender.com/health
```

---

## Troubleshooting

| Symptom | Possible Cause | Solution |
|---------|----------------|----------|
| `Connection refused` | Service not running | Start the service |
| `CORS error` | Frontend/backend mismatch | Check CORS config |
| `404 Not Found` | Wrong endpoint | Check API docs |
| `500 Internal Error` | Backend exception | Check backend logs |
| `403 Forbidden` | Missing token/wallet | Add auth headers |

---

## Test Data Cleanup

To reset the backend state:

```bash
# Remove persisted data
rm -f /tmp/store.json /tmp/events.json

# Restart backend
pkill -f uvicorn
uvicorn src.main:app --reload
```
