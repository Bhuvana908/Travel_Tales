🧳 Travel Tales — Trip Planning & Booking Demo

Travel Tales is a multi-page travel-planning web app that walks a user through signing in, choosing a destination and travel mode, browsing and "booking" outbound + return flights, picking a hotel, and viewing a final itinerary. The current build is modeled around a Hyderabad → Agra trip.

📘 Project Overview

The app is built as a sequence of static HTML pages (the "story" of a trip) backed by a few small, independent Express servers that talk to external APIs for real flight and hotel data, plus Firebase for authentication.

What it currently does:
- Lets a user sign in / register via Firebase Authentication
- Walks through choosing a trip type and mode of travel
- Shows flight options and a booking flow for an outbound trip
- Shows hotel options (e.g. Radisson Agra) and a booking flow
- Shows a trip summary, then repeats the flow for the return flight
- Ends on a final itinerary page
- Looks up live hotel data via the RapidAPI Hotels4 API
- Looks up live flight data via the Aviationstack API
- Sends a "welcome" email via Nodemailer when a user signs in

🧠 Key Concepts Covered
- Firebase Authentication (sign in / register)
- Multi-page static frontend flow (no SPA framework — plain HTML/CSS/JS)
- EJS templating for a server-rendered page
- REST API integration (RapidAPI, Aviationstack)
- Transactional email (Nodemailer + Gmail SMTP)
- Firebase Admin SDK on the backend

🗂️ Project Structure

```
Travel_Tales-main/
└── TRAVEL TALES/
    ├── travelhomepage.html, travelhomepages.html   # Landing pages
    ├── signin.html, register.html                  # Firebase Auth pages
    ├── choose.html, mode.html                       # Trip type / travel mode selection
    ├── national.html, seasonalplaces.html            # Destination browsing
    ├── flightdates.html, flightoptions.html,
    │   bookflight.html                               # Outbound flight (Hyderabad → Agra)
    ├── agrahotels.html, radisson.html,
    │   hotelsummary.html                             # Hotel browsing & booking (Agra)
    ├── summary.html                                  # Trip summary
    ├── ask.html, return.html, returnbook.html,
    │   returnsummary.html                             # Return flight (Agra → Hyderabad)
    ├── final.html, end.html                           # Final itinerary / closing page
    ├── map.html, maps.html                            # Map placeholder pages
    ├── flights.js                                     # Standalone Express server — flight search (Aviationstack)
    ├── server.js                                      # Firebase sign-in script, loaded by signin.html
    └── flights hotels/
        ├── app.js          # Express + EJS server — hotel search (RapidAPI Hotels4)
        ├── server.js       # Express server — sends a welcome email (Nodemailer + Firebase Admin)
        ├── index.ejs.html  # Hotel search results page (EJS view)
        ├── styles.css
        └── m.env.txt       # Holds the RapidAPI key — should be renamed to .env (see Security Notice)
```

There are three independent backend entry points (`flights.js`, `flights hotels/app.js`, `flights hotels/server.js`) — they aren't wired together yet, and there's no `package.json` declaring any dependencies.

⚙️ Tech Stack
- Frontend: Static HTML/CSS/vanilla JS, plus EJS for the hotel search page
- Backend: Node.js + Express
- Auth: Firebase Authentication (client) + Firebase Admin SDK (server)
- External APIs: RapidAPI — Hotels4 (hotel search), Aviationstack (flight data)
- Email: Nodemailer over Gmail SMTP

🔐 Security Notice — please read before your next commit

This repo currently has real, working credentials committed in plain text:
- A Firebase Web API key + project config, in `TRAVEL TALES/server.js`
- A Gmail address and app password, in `TRAVEL TALES/flights hotels/server.js`
- A RapidAPI key, in `TRAVEL TALES/flights hotels/m.env.txt`
- An Aviationstack API key, hardcoded in `TRAVEL TALES/flights.js`

Recommended fix, in order:
1. Rotate/revoke all four credentials now (Firebase API key restrictions, Gmail app password, RapidAPI key, Aviationstack key) — treat them as already public.
2. Move every key into a `.env` file and load it with `dotenv` instead of hardcoding it.
3. Rename `m.env.txt` → `.env`. The existing `.gitignore` already excludes `*.env`, but as a `.txt` file it is currently **not** excluded.
4. Use `git filter-repo` or BFG Repo-Cleaner to remove the secrets from git history — rotating the keys doesn't erase them from old commits.

🚧 Known Issues / In Progress
- No `package.json` yet — run `npm init -y` and install dependencies manually (see Setup below).
- `flights hotels/server.js` loads a Firebase service-account JSON via an absolute Windows path that only exists on the original dev machine — replace with a relative path to your own service-account file.
- `signin.html` loads `server.js` via an absolute Windows path instead of a relative one — update to a relative `<script src="server.js">` (or bundle it properly) once the auth script is wired into the page.
- All three backend servers default to port 3000 — change the port in at least two of them before running more than one at a time.
- The RapidAPI Hotels4 key needs an active subscription on RapidAPI, or hotel search will fail/return empty results.
- `map.html` / `maps.html` are placeholder pages.
- Booking data currently doesn't persist between pages (no shared session or database) — selections made on one page won't carry over to the next yet.

🛠️ Setup & Usage

Prerequisites
- Node.js (v16+)
- A Firebase project with Authentication enabled
- A RapidAPI account subscribed to the Hotels4 API
- An Aviationstack API key
- A Gmail account with an App Password (for Nodemailer)

1. Clone and install dependencies
```
git clone https://github.com/Bhuvana908/Travel_Tales.git
cd "Travel_Tales/TRAVEL TALES"
npm init -y
npm install express cors body-parser nodemailer firebase-admin axios dotenv node-fetch ejs
```

2. Configure environment variables

Create a `.env` file inside `TRAVEL TALES/flights hotels/`:
```
RAPIDAPI_KEY=your_rapidapi_key_here
```

Keep your Gmail app password and Aviationstack key in your own untracked `.env` file(s) as well — don't hardcode them in `server.js` / `flights.js` as they currently are.

3. Run the servers (one at a time, until ports are split out)
```
# Hotel search (EJS-rendered)
node "flights hotels/app.js"      # http://localhost:3000

# Welcome-email service
node "flights hotels/server.js"   # http://localhost:3000 — change this port before running alongside app.js

# Flight search API
node flights.js                    # http://localhost:3000 — change this port too
```

4. Open the frontend

Open `travelhomepage.html` directly in a browser, or serve the folder with a static server, e.g.:
```
npx serve "TRAVEL TALES"
```

🗺️ User Flow

`travelhomepage.html` → `signin.html` / `register.html` → `choose.html` → `mode.html` → `flightdates.html` → `flightoptions.html` → `bookflight.html` → `agrahotels.html` → `radisson.html` → `hotelsummary.html` → `summary.html` → `ask.html` → `return.html` → `returnbook.html` → `returnsummary.html` → `final.html` → `end.html`

📌 Notes
- This is currently a frontend-first demo: the booking steps are static pages that don't yet persist state to Firestore or a shared session — that's a natural next step.
- The hotel search page (`flights hotels/index.ejs.html`) is the only page rendered server-side via EJS; everything else is plain static HTML.
