# TRIPnVIBE ✈️🗺️

An intelligent, full-stack travel planning and itinerary optimization platform. TRIPnVIBE helps travelers craft personalized trips, discover places, optimize travel routes using Traveling Salesperson Problem (TSP) algorithms, book services, and collaborate in real-time.

---

## 🌟 Key Features

- **📍 Smart Route & Itinerary Optimization**: 
  - Automated stop-order optimization using a **Nearest Neighbor TSP** heuristic.
  - Integration with **Google Distance Matrix API** for real driving times and distances, with automatic **Haversine formula fallback**.
- **🗺️ Interactive Map Exploration**: 
  - Dynamic mapping powered by **Leaflet** and **OpenStreetMap** with markers, routes, and waypoints.
- **🏨 Seamless Booking Integrations**:
  - Support for multi-modal travel bookings: Flights (Amadeus API), Trains (IRCTC), and Cabs/Rides (Uber).
- **💳 Secure Payments**:
  - Full payment processing workflow powered by **Stripe** with invoices and webhook handling.
- **⚡ Real-Time Collaboration**:
  - **WebSocket** service for live group trip planning and synchronized itinerary updates.
- **🔐 Robust Authentication & Security**:
  - **JWT (JSON Web Tokens)** authentication, OAuth2 password flow, and **bcrypt** password hashing.

---

## 🛠️ Tech Stack

### Backend
- **Framework**: [FastAPI](https://fastapi.tiangolo.com/) (Python 3.10+)
- **Database & ORM**: [SQLAlchemy 2.0](https://www.sqlalchemy.org/) with PostgreSQL / SQLite support
- **Data Validation**: [Pydantic v2](https://docs.pydantic.dev/) & Pydantic Settings
- **Async & Realtime**: [WebSockets](https://websockets.readthedocs.io/), [Uvicorn](https://www.uvicorn.org/)
- **Payments**: [Stripe Python SDK](https://stripe.com/docs/api)
- **HTTP Client**: [HTTPX](https://www.python-httpx.org/)

### Frontend
- **Framework**: [React 19](https://react.dev/) + [TypeScript](https://www.typescriptlang.org/)
- **Build Tool**: [Vite](https://vitejs.dev/)
- **Routing**: [React Router v7](https://reactrouter.com/)
- **Mapping**: [Leaflet](https://leafletjs.com/) & [React-Leaflet](https://react-leaflet.js.org/)
- **Icons**: [Lucide React](https://lucide.dev/)
- **HTTP Client**: [Axios](https://axios-http.com/)

---

## 📂 Project Structure

```text
TripNVibe/
├── backend/
│   ├── app/
│   │   ├── api/
│   │   │   ├── endpoints/       # API route handlers (auth, users, trips, places, bookings, payments, ws)
│   │   │   ├── api.py           # Unified router aggregator
│   │   │   └── deps.py          # FastAPI dependencies (DB session, current user)
│   │   ├── core/
│   │   │   ├── config.py        # Environment settings & configuration
│   │   │   └── security.py      # JWT token generation & password hashing
│   │   ├── crud/                # Database CRUD operations
│   │   ├── db/                  # Engine, session, and declarative Base
│   │   ├── models/              # SQLAlchemy database models
│   │   ├── schemas/             # Pydantic request/response schemas
│   │   ├── services/            # TSP solver, Google Maps, Stripe, WebSockets
│   │   └── main.py              # Application entry point & CORS configuration
│   └── requirements.txt         # Python dependencies
│
├── frontend/
│   ├── src/
│   │   ├── api/                 # Axios API client & endpoints
│   │   ├── pages/               # React views (Landing, Login, Dashboard, Planner, Booking, Payment, etc.)
│   │   ├── App.tsx              # Application root & routes
│   │   └── main.tsx             # Vite React entry point
│   ├── package.json             # Node dependencies and scripts
│   └── vite.config.ts           # Vite build & development configuration
│
└── README.md
```

---

## 🚀 Getting Started

### Prerequisites
- **Python 3.10+**
- **Node.js 18+** and **npm** (or pnpm / yarn)
- **PostgreSQL** (optional, defaults to SQLite for local development)

---

### 1. Backend Setup

1. **Navigate to the backend directory:**
   ```bash
   cd backend
   ```

2. **Create and activate a virtual environment:**
   ```bash
   # Windows (PowerShell)
   python -m venv venv
   .\venv\Scripts\Activate.ps1

   # macOS / Linux
   python3 -m venv venv
   source venv/bin/activate
   ```

3. **Install dependencies:**
   ```bash
   pip install -r requirements.txt
   ```

4. **Configure environment variables:**
   Create a `.env` file in `backend/` with the following parameters:
   ```env
   PROJECT_NAME="TRIPnVIBE"
   SECRET_KEY="your-super-secret-jwt-key"
   ACCESS_TOKEN_EXPIRE_MINUTES=11520

   # Database (leave default or point to PostgreSQL)
   POSTGRES_SERVER="localhost"
   POSTGRES_USER="postgres"
   POSTGRES_PASSWORD="yourpassword"
   POSTGRES_DB="tripnvibe"

   # Optional External Integrations
   GOOGLE_MAPS_API_KEY=""
   STRIPE_SECRET_KEY=""
   STRIPE_WEBHOOK_SECRET=""
   AMADEUS_API_KEY=""
   IRCTC_API_KEY=""
   UBER_API_KEY=""
   ```

5. **Start the backend development server:**
   ```bash
   uvicorn app.main:app --reload --host 0.0.0.0 --port 8000
   ```
   - API Root: `http://localhost:8000`
   - Interactive Swagger Docs: `http://localhost:8000/docs`
   - ReDoc: `http://localhost:8000/redoc`

---

### 2. Frontend Setup

1. **Navigate to the frontend directory:**
   ```bash
   cd frontend
   ```

2. **Install dependencies:**
   ```bash
   npm install
   ```

3. **Start the development server:**
   ```bash
   npm run dev
   ```
   Open your browser and navigate to `http://localhost:5173`.

---

## 📡 API Endpoints Overview

| Category | Method | Endpoint | Description |
| :--- | :--- | :--- | :--- |
| **Auth** | `POST` | `/api/v1/login/access-token` | User login & JWT issuance |
| **Users** | `POST` | `/api/v1/users/` | Register new user account |
| **Users** | `GET` | `/api/v1/users/me` | Fetch authenticated user profile |
| **Trips** | `GET` | `/api/v1/trips/` | Retrieve user trips |
| **Trips** | `POST` | `/api/v1/trips/` | Create a new trip |
| **Trips** | `POST` | `/api/v1/trips/{id}/optimize` | Optimize trip route using TSP |
| **Places** | `GET` | `/api/v1/places/search` | Search destinations & tourist attractions |
| **Bookings**| `POST` | `/api/v1/bookings/` | Create flight, hotel, or ride reservation |
| **Payments**| `POST` | `/api/v1/payments/create-intent` | Initialize Stripe payment session |
| **WebSockets**| `WS` | `/api/v1/ws/{trip_id}` | Real-time collaborative trip session |

---

## 👥 Contributors

- **P Hemanth Kumar Reddy** ([@hemanthkumarrrdy-bit](https://github.com/hemanthkumarrrdy-bit))
- **Polakala Madhurima** ([@polakalamadhurima-dotcom](https://github.com/polakalamadhurima-dotcom))
