# 🌍 Travel Tales

> Plan your entire trip — transport, stay, sightseeing, and your guide — in one place.

Travel Tales is an all-in-one trip planning platform built for travelers who don't want to juggle five different apps to book a vacation. Instead of separately booking flights, hunting for hotels, building an itinerary, and arranging a local guide, Travel Tales pulls everything together: tickets, accommodation, day-by-day plans, guide details, and a single combined cost — all from one dashboard.

---

## ✨ What It Does

| Module | What it handles |
|---|---|
| 🚆 **Transport** | Search and book flights, buses, or trains for your route |
| 🏨 **Stay** | Find and reserve hotels based on location, budget, and preferences |
| 🗺️ **Itinerary** | Auto-builds a day-wise schedule — arrival/departure, check-in/check-out, and timed slots for each attraction |
| 🧑‍🏫 **Local Guide** | Shows who's accompanying you at each destination, with their details |
| 💰 **Trip Cost** | Adds up transport + hotel + guide fees into one final number |
| ⚡ **Minimal Input** | You give the basics (where, when, budget) — the platform does the rest |

The goal is simple: type in a destination and a few preferences, and walk away with a ready-to-go trip plan instead of ten open browser tabs.

---

## 🛠️ Built With

**Client side:** HTML, CSS, JavaScript, React
**Server side:** Node.js with Express *(work in progress)*
**Data storage:** MongoDB / Firebase / MySQL (depending on environment)
**External data:** Third-party APIs for transport, hotels, and guide listings
**Deployment:** Vercel, Netlify, or Heroku — your pick

---

## 📁 How the Repo Is Laid Out

```
Travel-Tales/
├── (frontend + partial backend source)
├── README.md
└── .gitignore
```

---

## 🚀 Getting It Running Locally

**1. Grab the code**
```bash
git clone https://github.com/yourusername/travel-tales.git
cd travel-tales
```

**2. Set up the client**
```bash
cd frontend
npm install
npm start
```

**3. Set up the server** *(still being built out)*
```bash
cd backend
npm install
```

**4. Add your environment config**

Inside the `backend` folder, create a `.env` file:
```
DB_URL=your_database_url
API_KEY_TRANSPORT=your_transport_api_key
API_KEY_HOTELS=your_hotel_api_key
```

**5. Start the backend**
```bash
npm start
```

Then head to `http://localhost:3000` (or whatever port you've configured) to see it live.

---

## 🧭 Why This Exists

Booking a trip usually means stitching together a flight site, a hotel site, a separate guide/tour booking, and a spreadsheet to track what it's all costing. Travel Tales folds that whole process into one flow — so the planning takes minutes, not hours.
