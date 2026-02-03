# Demo Guide

Complete walkthrough for demonstrating the Microservices Factory.

---

## Prerequisites

Before the demo:
- Backend running on `localhost:8000`
- Frontend running on `localhost:3000`
- OR use live demo: [team-microservices-factory-gamma.vercel.app](https://team-microservices-factory-gamma.vercel.app) (requires local backend)

---

## Demo Script

### Scene 1: Introduction (30 seconds)

**Talking Points:**
> "This is the Microservices Factory - a platform where you describe what you want, and AI agents build and deploy it for you. Each service gets its own token, and holding the token grants API access."

**Show:**
- Landing page with title "Microservices Factory"
- Explain the three main sections: Wallet, Submit Idea, Services

---

### Scene 2: Connect Wallet (15 seconds)

**Action:**
1. Scroll to "Wallet Connection" panel
2. Enter a valid Ethereum address:
   ```
   0x742d35Cc6634C0532925a3b844Bc9e7595f2bD10
   ```
3. Show "Wallet connected" confirmation

**Talking Points:**
> "First, connect your wallet. This address will be associated with your services and used for token-gated API access."

---

### Scene 3: Submit an Idea (30 seconds)

**Action:**
1. Scroll to "Submit Idea" panel
2. Type an example idea:
   ```
   A service that takes a URL and returns a summary 
   of the webpage content in 3 bullet points
   ```
3. Click "Submit"
4. Show the service appearing in the Services list

**Talking Points:**
> "Describe what you want in plain English. Our agents will interpret this and generate a working microservice."

---

### Scene 4: View Service Status (20 seconds)

**Action:**
1. Point to the new service card
2. Highlight:
   - Service ID (unique identifier)
   - Status (queued -> generating -> deployed)
   - Created/Updated timestamps

**Talking Points:**
> "Each service gets a unique ID and progresses through stages: queued, generating code, deploying, and finally deployed."

---

### Scene 5: Platform Stats (15 seconds)

**Action:**
1. Scroll to show the Stats panel
2. Point out:
   - Total Services count
   - Deployed count
   - Tokenized count

**Talking Points:**
> "The platform tracks all services. You can see how many are deployed and tokenized across the system."

---

### Scene 6: Deploy Service (20 seconds)

**Action:**
1. Click "Deploy" button on a service
2. Wait for status to update
3. Show the API URL appear

**Talking Points:**
> "Clicking Deploy triggers the agent to package and deploy your service to the cloud. You'll get a live API endpoint."

---

### Scene 7: Create Token (20 seconds)

**Action:**
1. Click "Create Token" button
2. Show the token address appear

**Talking Points:**
> "Each service gets its own ERC20 token with a bonding curve. The token price increases as more people buy, creating a market for popular services."

---

### Scene 8: Get API Access (20 seconds)

**Action:**
1. Click "Get Access" button
2. Show the access info:
   - API base URL
   - Token address
   - API key

**Talking Points:**
> "To use the API, you need to hold the service's token. This creates a self-sustaining model where service creators benefit from usage."

---

### Scene 9: API Demo (30 seconds)

**Action:**
Show terminal with curl command:
```bash
curl -X POST https://your-service.vercel.app/process \
  -H "Content-Type: application/json" \
  -H "X-Wallet-Address: 0x742d35Cc6634C0532925a3b844Bc9e7595f2bD10" \
  -d '{"input": "https://example.com"}'
```

**Talking Points:**
> "The deployed service exposes a standard API. Pass your wallet address in the header, and the gateway verifies you hold the token before forwarding your request."

---

### Scene 10: Architecture Overview (30 seconds)

**Show diagram:**
```
User -> Frontend -> Backend -> [Generate Code] -> [Deploy]
                        |
                        v
                    Gateway <- Token Check <- Blockchain
                        |
                        v
                  Deployed Service
```

**Talking Points:**
> "The architecture is simple but powerful:
> 1. Frontend collects ideas
> 2. Backend orchestrates generation and deployment
> 3. Gateway enforces token-gating
> 4. Smart contracts handle the token economics"

---

## Q&A Responses

**Q: How does the bonding curve work?**
> The token price increases linearly with supply. Early buyers get lower prices. When you sell, you get paid based on current supply.

**Q: What chains are supported?**
> Currently targeting Ethereum testnets (Sepolia). Base and other L2s planned.

**Q: How do agents generate code?**
> The backend uses LLM-based generation with FastAPI templates. Generated services follow a standard structure.

**Q: What happens if a service fails?**
> Status updates to "failed" with error message. Users can retry or submit a new idea.

---

## Troubleshooting Demo Issues

| Issue | Solution |
|-------|----------|
| "Backend API unavailable" | Start backend: `cd backend && uvicorn src.main:app` |
| Services not loading | Click Refresh button |
| Token creation fails | Service must be deployed first |
| Access creation fails | Token must be created first |

---

## Demo Checklist

Before presenting:
- [ ] Backend is running
- [ ] Frontend is accessible
- [ ] Test wallet address ready
- [ ] Sample ideas prepared
- [ ] Terminal ready for API demo
