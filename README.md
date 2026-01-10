# 💳 Payment Gateway – Multi-Method Processing with Hosted Checkout

A production-style payment gateway inspired by Razorpay/Stripe, supporting **UPI** and **Card** payments with a clean **hosted checkout** and **merchant dashboard**.

---

## ✨ Features

* ✅ **RESTful APIs** – Orders, payments, and status tracking
* ✅ **Multiple Payment Methods** – UPI & Cards with strong validation
* ✅ **Hosted Checkout** – Secure, professional payment experience
* ✅ **Merchant Dashboard** – Login, API keys, transactions, analytics
* ✅ **Authentication** – API Key & Secret for merchants
* ✅ **Database Persistence** – PostgreSQL with indexes & relations
* ✅ **Dockerized Setup** – One-command deployment
* ✅ **Test Mode** – Deterministic payment outcomes for testing

---

## 🏗️ System Architecture

```
┌─────────────┐         ┌─────────────┐         ┌──────────────┐
│  Dashboard  │────────▶│   Backend   │────────▶│  PostgreSQL  │
│  (3000)     │         │   API       │         │  Database    │
└─────────────┘         │  (8000)     │         └──────────────┘
                        └─────────────┘
                              ▲
                              │
                        ┌─────────────┐
                        │  Checkout   │
                        │  (3001)     │
                        └─────────────┘
```

---

## 📦 Tech Stack

* **Backend**: Node.js, Express.js, TypeORM
* **Database**: PostgreSQL 15
* **Frontend**: React 18, React Router, Axios
* **Styling**: CSS Variables, Responsive UI
* **Infrastructure**: Docker & Docker Compose
* **Auth**: API Key / Secret pattern

---

## 🚀 Quick Start

### Prerequisites

* Docker & Docker Compose
* Git
* Free ports: `3000`, `3001`, `8000`, `5432`

### Installation

```bash
# Clone repository
git clone https://github.com/srujanaA02/payment-gateway.git
cd payment-gateway

# Start all services
docker-compose up -d

# Verify services
docker ps
curl http://localhost:8000/health
```

### Application URLs

* **Merchant Dashboard**: [http://localhost:3000/login](http://localhost:3000/login)
* **Hosted Checkout**: [http://localhost:3001/checkout?order_id=](http://localhost:3001/checkout?order_id=)<ORDER_ID>
* **Backend API**: [http://localhost:8000](http://localhost:8000)
* **Health Check**: [http://localhost:8000/health](http://localhost:8000/health)

---

## 🔐 Test Credentials

```
Email: test@example.com
Password: any

API Key: key_test_abc123
API Secret: secret_test_xyz789
```

---

## 📚 API Overview

### Authentication Headers

```
X-Api-Key: <API_KEY>
X-Api-Secret: <API_SECRET>
```

### Core Endpoints

| Method | Endpoint             | Description          |
| ------ | -------------------- | -------------------- |
| GET    | /health              | Service health check |
| POST   | /api/v1/orders       | Create an order      |
| GET    | /api/v1/orders/:id   | Fetch order          |
| POST   | /api/v1/payments     | Create payment       |
| GET    | /api/v1/payments/:id | Fetch payment        |

### Public Checkout APIs

```
GET  /api/v1/orders/:id/public
POST /api/v1/payments/public
```

---

## 🧾 Database Schema

### Merchants

* `id (UUID)`
* `email`, `name`
* `api_key`, `api_secret`
* `is_active`, timestamps

### Orders

* `id (order_xxx)`
* `merchant_id`
* `amount`, `currency`
* `receipt`, `notes`
* `status`, timestamps

### Payments

* `id (pay_xxx)`
* `order_id`, `merchant_id`
* `amount`, `method`, `status`
* `vpa`, `card_network`, `card_last4`
* `error_code`, timestamps

---

## ✅ Payment Validation

### UPI

* Regex validation for VPA format

### Card

* **Luhn Algorithm** for card number
* Network detection: Visa, Mastercard, Amex, RuPay
* Expiry date validation
* Only last 4 digits stored (PCI-safe demo)

---

## 🧪 Testing Flow

```bash
# Create order
curl -X POST http://localhost:8000/api/v1/orders \
 -H "X-Api-Key: key_test_abc123" \
 -H "X-Api-Secret: secret_test_xyz789" \
 -H "Content-Type: application/json" \
 -d '{"amount":50000,"currency":"INR"}'

# Open checkout
http://localhost:3001/checkout?order_id=<ORDER_ID>
```

**Test Card**: `4111 1111 1111 1111`
**Expiry**: `12/26`
**CVV**: `123`

---

## 🐳 Docker Commands

```bash
docker-compose up -d

docker-compose logs -f

docker-compose down
```

---

## 📁 Project Structure

```
payment-gateway/
├── backend/
├── frontend/
├── checkout-page/
├── docker-compose.yml
├── .env.example
└── README.md
```

---

## 🧠 Notes

* This project is **educational & demo-grade** (not PCI compliant)
* Designed to showcase **payment gateway fundamentals**
* Easily extensible with webhooks, refunds, and settlements

---

## 🎥 Demo Video

A complete end-to-end walkthrough of the payment flow (Order → Checkout → Payment → Status) is available below:

🔗 **Video Demo**: [https://docs.google.com/videos/d/18C_7jD4WDQ625j_BptaTl8mxAQJEDZyFc-OTHVpG154/edit?usp=sharing](https://docs.google.com/videos/d/18C_7jD4WDQ625j_BptaTl8mxAQJEDZyFc-OTHVpG154/edit?usp=sharing)

This demo showcases:

* Merchant dashboard usage
* Order creation via API
* Hosted checkout experience
* UPI & Card payment flows
* Success & failure handling

---

**⭐ If you find this useful, give the repository a star!**
