# Telkes-Gateway
Noncustodial-gateway or open-pay-gateway for persons wanting to accept peer to peer payments 
🚦 Non-custodial Gateway

Open-source multi-rail payment gateway.
Supports Stripe (Connect), PayPal Orders + Payouts, BTCPay Server (BTC/LN), and EVM (USDC/ETH) — with optional Solidity escrow.

👉 Your system never holds funds. All payments route directly between payers and recipients.

✨ Features

🔒 Non-custodial by design — no balance storage, no custody risk

💳 Stripe Connect (hosted checkout → direct to recipient)

🅿️ PayPal Orders + Payouts (checkout + direct payouts to emails)

₿ BTCPay Server (self-hosted Bitcoin/Lightning invoices)

⛽ EVM rail (WalletConnect/MetaMask payloads for USDC or ETH)

📜 Optional Solidity escrow for hold + release flows

🗄️ Postgres + Prisma schema for payments + events

📂 File tree
noncustodial-gateway/
├─ package.json
├─ .env.example
├─ prisma/schema.prisma
├─ contracts/Escrow.sol
├─ src/
│  ├─ server.js
│  ├─ recipients.js
│  ├─ db/prisma.js
│  ├─ rails/
│  │  ├─ stripe.js
│  │  ├─ paypal.js
│  │  ├─ btcpay.js
│  │  └─ evm.js
│  └─ webhooks/
│     ├─ stripe.js
│     ├─ paypal.js
│     └─ btcpay.js

⚙️ Setup
git clone https://github.com/yourusername/noncustodial-gateway.git
cd noncustodial-gateway
npm install
cp .env.example .env   # fill with your keys
npx prisma generate
npx prisma db push
npm run dev

🌍 Environment

Fill in your .env:

PORT=3000
CORS_ORIGIN=http://localhost:5173

# Stripe
STRIPE_SECRET_KEY=sk_test_...
STRIPE_WEBHOOK_SECRET=whsec_...

# PayPal
PAYPAL_CLIENT_ID=...
PAYPAL_CLIENT_SECRET=...
PAYPAL_MODE=sandbox
PAYPAL_WEBHOOK_ID=...

# BTCPay
BTCPAY_URL=https://your-btcpay.example
BTCPAY_API_KEY=...
BTCPAY_STORE_ID=...

# Database
DATABASE_URL=postgresql://user:pass@localhost:5432/gateway?schema=public

# EVM
RPC_URL=https://polygon-mumbai.infura.io/v3/...
USDC_CONTRACT=0x...
ESCROW_CONTRACT=0xYourEscrowContract
EVM_CHAIN_ID=80001

🔌 Usage
Create payment
curl -X POST http://localhost:3000/pay \
 -H "Content-Type: application/json" \
 -d '{
   "amount": 2500,
   "currency": "USD",
   "rail": "paypal",
   "recipientId": "r_paypal_1",
   "description": "Order #1001"
 }'

Webhooks

POST /webhooks/stripe

POST /webhooks/paypal

POST /webhooks/btcpay

PayPal Payout
curl -X POST http://localhost:3000/payouts/paypal \
 -H "Content-Type: application/json" \
 -d '{
   "amount": 1000,
   "currency": "USD",
   "receiverEmail": "payouts@example.com",
   "note": "Creator revenue share"
 }'

🛠 Tech stack

Node.js (Express)

Stripe, PayPal SDKs, BTCPay REST

Ethers.js for EVM payloads

Prisma + Postgres for persistence

📜 License

MIT — free to fork, adapt, and extend.
