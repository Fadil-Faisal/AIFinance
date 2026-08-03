<div align="center">

# 💰 AI Finance Manager



A full-stack personal finance platform that auto-categorizes spending, forecasts future expenses, and tracks budgets and market data — all in one place.

[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-0.104-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=black)](https://react.dev/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=for-the-badge&logo=postgresql&logoColor=white)](https://www.postgresql.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg?style=for-the-badge)](LICENSE)

</div>

---

## 📖 About The Project

Manually tagging every expense and guessing at next month's spending is tedious and easy to abandon. **AI Finance Manager** takes that friction out: transactions are auto-categorized with a trained ML model as soon as they're logged, a prediction model forecasts upcoming expenses from historical trends, and budgets alert you before you overspend — alongside live currency and market data in the same platform.

The project is a full-stack app: a FastAPI backend that owns auth, data, and the ML models, and a separate React + TypeScript frontend that consumes it.

---

## 🛠️ Built With

| Category | Stack |
|---|---|
| **Backend** | ![Python](https://img.shields.io/badge/Python-3776AB?style=flat-square&logo=python&logoColor=white) ![FastAPI](https://img.shields.io/badge/FastAPI-009688?style=flat-square&logo=fastapi&logoColor=white) |
| **Database & ORM** | ![PostgreSQL](https://img.shields.io/badge/PostgreSQL-4169E1?style=flat-square&logo=postgresql&logoColor=white) SQLAlchemy, Alembic |
| **Auth & Security** | JWT (python-jose), Passlib + bcrypt, Pydantic validation |
| **Machine Learning** | scikit-learn (TF-IDF + Multinomial Naive Bayes), pandas, NumPy, NLTK |
| **External APIs** | Frankfurter (currency), Alpha Vantage (stocks), CoinGecko (crypto) |
| **Frontend** | ![React](https://img.shields.io/badge/React-61DAFB?style=flat-square&logo=react&logoColor=black) ![TypeScript](https://img.shields.io/badge/TypeScript-3178C6?style=flat-square&logo=typescript&logoColor=white) Material UI (MUI), TanStack Query |
| **Frontend Charts/Calendar** | Chart.js, Recharts, MUI X Charts, FullCalendar |
| **Frontend Other** | Axios, React Hook Form, Framer Motion, React Toastify |
| **Testing** | pytest, pytest-cov, React Testing Library |
| **Deployment** | Gunicorn + Uvicorn workers, Docker |

---

## ✨ Features

**🔐 User Management** — JWT-based auth, bcrypt password hashing, per-user preferred currency

**💳 Accounts** — Checking, Savings, Credit Card, Investment, and Cash accounts with real-time balances and multi-currency support

**📊 Transactions** — AI-powered auto-categorization (TF-IDF + Naive Bayes) on top of 10+ predefined categories, with filtering, pagination, and summaries

**🎯 Budgets** — Category-level spending limits (weekly/monthly/yearly) with configurable alert thresholds and live spent/remaining tracking

**🤖 Machine Learning** — Expense categorization, spending predictions (moving averages + trend projection), and trend analysis, each with a confidence score

**💱 Currency Exchange** — Free, real-time ECB rates across 30+ currencies via the Frankfurter API — no key required

**📈 Market Data** — Real-time stock prices (Alpha Vantage) and crypto prices (CoinGecko), cached for 5 minutes

---

## 🚀 Getting Started

### Prerequisites

- Python 3.9+
- PostgreSQL
- Node.js 18+ (for the frontend)
- pip

### Backend setup

```bash
git clone https://github.com/Fadil-Faisal/AIFinance.git
cd AIFinance

# Create and activate a virtual environment
python -m venv venv
source venv/bin/activate      # macOS/Linux
venv\Scripts\activate         # Windows

# Install dependencies
pip install -r requirements.txt

# Configure environment
cp .env.example .env
```

Edit `.env`:

```env
DATABASE_URL=postgresql://postgres:password@localhost:5432/finance_db
SECRET_KEY=your-secret-key-here-change-in-production-min-32-chars

# Optional — for market data
ALPHA_VANTAGE_API_KEY=your_key_here
COINGECKO_API_KEY=your_key_here
```

```bash
# Create the database
createdb finance_db

# Run the API (tables are created automatically on first run)
uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
```

- API docs: **http://localhost:8000/docs**
- Alternative docs: **http://localhost:8000/redoc**

### Frontend setup

```bash
cd finance-frontend
npm install
npm start
```

Open **http://localhost:3000**.

---

## 💻 Usage

**Register a user:**

```bash
curl -X POST http://localhost:8000/api/v1/auth/register \
  -H "Content-Type: application/json" \
  -d '{
    "email": "user@example.com",
    "password": "securepass123",
    "full_name": "John Doe",
    "preferred_currency": "USD"
  }'
```

**Log a transaction (auto-categorized by the ML model):**

```bash
curl -X POST http://localhost:8000/api/v1/transactions \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{
    "account_id": 1,
    "amount": 45.50,
    "transaction_type": "expense",
    "description": "Coffee at Starbucks",
    "transaction_date": "2026-08-01"
  }'
```

The description alone is enough — the model tags this as `"Food"` automatically.

**Get budget status:**

```json
GET /api/v1/budgets/status
→
[
  {
    "budget": { "category": "Food", "limit_amount": 500.00, "period": "monthly" },
    "spent": 387.50,
    "remaining": 112.50,
    "percentage_used": 77.50,
    "alert": false
  }
]
```

**Predict next month's spending:**

```bash
curl -X POST http://localhost:8000/api/v1/ml/predict-expenses \
  -H "Authorization: Bearer $TOKEN" \
  -H "Content-Type: application/json" \
  -d '{"months_ahead": 1}'
```

---

## 🤖 Machine Learning Details

| Model | Approach |
|---|---|
| **Expense categorization** | TF-IDF + Multinomial Naive Bayes, trained on 200+ labeled transactions, unigrams + bigrams, ~85–90% test accuracy |
| **Expense prediction** | Moving averages + linear trend projection; needs 5+ transactions minimum; confidence rises with transaction volume (High: 50+, Medium: 20–49, Low: <20) |

---

## 🗺️ Roadmap

- [ ] Bring frontend test coverage up to match the backend's pytest suite
- [ ] Add refresh tokens / longer-lived sessions
- [ ] Swap the statistical predictor for a proper time-series model (e.g., Prophet)
- [ ] Add CI (lint + test) for both backend and frontend
- [ ] Dockerize the frontend alongside the existing backend Dockerfile
- [ ] Add data export (CSV/PDF) for transactions and budgets

See [open issues](https://github.com/Fadil-Faisal/AIFinance/issues) for the full list.

---

## 🔒 Security

- Passwords hashed with bcrypt (cost factor 12)
- JWT-based authentication with configurable expiration
- CORS protection, SQL-injection protection via SQLAlchemy ORM, Pydantic input validation

---

## 🧪 Testing

```bash
pytest                        # run all tests
pytest --cov=app tests/       # with coverage
pytest tests/test_auth.py     # a specific file
```

---

## ☁️ Deployment

```bash
# Production server
gunicorn app.main:app -w 4 -k uvicorn.workers.UvicornWorker --bind 0.0.0.0:8000
```

A `Dockerfile`-ready setup for the backend is included — see the repo for details.

---

## 🤝 Contributing

Contributions are welcome!

1. Fork the repository
2. Create a feature branch (`git checkout -b feat/your-feature`)
3. Commit your changes
4. Push and open a Pull Request

---

## 📄 License

Distributed under the MIT License. See `LICENSE` for details.

---

## 📬 Contact

**Fadil Faisal** — [GitHub](https://github.com/Fadil-Faisal)

Project Link: [https://github.com/Fadil-Faisal/AIFinance](https://github.com/Fadil-Faisal/AIFinance)

---

## 🙏 Acknowledgements

- FastAPI for the backend framework
- scikit-learn for the ML pipeline
- Frankfurter API for free currency data
- Alpha Vantage and CoinGecko for market data

</div>
