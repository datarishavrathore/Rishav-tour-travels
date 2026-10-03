# ✈️ Rishav Tour & Travels

A travel-agency web application built with **Flask** (frontend / web layer) that talks to a separate **FastAPI backend** for authentication and customer data. Styled with **Tailwind CSS** and Jinja2 templates.

## ✨ Features

- 🏠 **Landing page** – modern marketing home page for the travel business
- 📝 **Customer registration** – signup form (name, email, phone, password) posted to the backend
- 🔐 **Login / Logout** – JWT-based authentication; token is stored in the Flask session
- 👤 **Customer dashboard** – personalised area for logged-in customers
- 🛠️ **Admin dashboard** – business overview (KPIs, revenue chart, upcoming trips, recent bookings, top destinations)
- 📋 **Customer directory** – admin can view all registered customers (fetched from the backend with the Bearer token)
- ❤️ **Health check** – `/health` route to confirm the Flask app can reach the FastAPI backend

> **Note:** The dashboard widgets (revenue, bookings, trips, etc.) currently show static demo data. Real data wiring is a planned improvement.

## 🏗️ Architecture

![Architecture Diagram](docs/architecture.png)

The system has three logical parts:

| Layer | Description |
|-------|-------------|
| **Web Interface** | Flask routes in `app.py` render Jinja2 templates (`home`, `customer_dashboard`, `admin_dashboard`, all extending `base.html`). |
| **Authentication** | Registration and login flows in `app.py` render `register.html` / `login.html` and POST credentials to the FastAPI backend. After login, users are redirected to the admin or customer dashboard. |
| **Customer Access** | The `/customers` route sends the stored JWT to the backend (`GET /users/`) and renders `customer_list.html`. |

### Request flow

1. A visitor opens the site → Flask serves `home.html`.
2. **Signup:** form → Flask → `POST {FASTAPI_URL}auth/register` → redirect to login.
3. **Login:** form → Flask → `POST {FASTAPI_URL}auth/login` → backend returns `access_token` + `full_name` → saved in session → redirect to admin or customer dashboard.
4. **Customers list:** Flask reads the token from session → `GET {FASTAPI_URL}users/` with `Authorization: Bearer <token>` → renders the list. A `401` clears the session; a `403` redirects home.

## 📁 Project Structure

```
rishav-tour-travels/
├── app.py                  # Flask app & all routes
├── requirements.txt        # Python dependencies
├── docs/
│   └── architecture.png    # Architecture diagram
└── templates/
    ├── base.html           # Shared layout (navbar, Tailwind, fonts)
    ├── home.html           # Landing page
    ├── login.html          # Login page
    ├── register.html       # Registration page
    ├── customer_dashboard.html
    ├── admin_dashboard.html
    └── customer_list.html  # Customer directory
```

## 🛣️ Routes

| Route | Method | Description |
|-------|--------|-------------|
| `/` | GET | Home page |
| `/signup` | GET, POST | Register a new customer |
| `/login` | GET, POST | Authenticate user |
| `/logout` | GET | Clear session |
| `/customers` | GET | List customers (requires login) |
| `/admin-dashboard` | GET | Admin dashboard |
| `/customer-dashboard` | GET | Customer dashboard |
| `/health` | GET | Check backend connectivity |

## 🚀 Getting Started

### Prerequisites
- Python 3.9+
- The FastAPI backend running and reachable (exposing `/health`, `/auth/register`, `/auth/login`, `/users/`)

### Installation

```bash
git clone https://github.com/datarishavrathore/rishav-tour-travels.git
cd rishav-tour-travels

python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

pip install -r requirements.txt
```

### Configuration

Create a `.env` file in the project root:

```env
SECRET_KEY=your-strong-random-secret
FASTAPI_URL=http://localhost:8001/
```

> `FASTAPI_URL` must end with a trailing `/` because routes are appended directly (e.g. `auth/login`).

### Run

```bash
python app.py
```

The app starts at **http://localhost:8000**.

## 🧰 Tech Stack

- **Frontend / Web:** Flask, Jinja2, Tailwind CSS (CDN)
- **Backend API:** FastAPI (separate service)
- **Auth:** JWT (Bearer token) stored in Flask session
- **HTTP client:** `requests`
- **Config:** `python-dotenv`

## 🔮 Roadmap

- [ ] Role-based access control driven by the backend (not hard-coded)
- [ ] Protect `/admin-dashboard` and `/customer-dashboard` with login checks
- [ ] Show error messages on failed login / signup
- [ ] Real booking, package and trip management
- [ ] Live dashboard analytics from the database

## 👨‍💻 Author

**Rishav Rathore** — [@datarishavrathore](https://github.com/datarishavrathore)

---
⭐ If you like this project, give it a star!
