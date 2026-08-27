# 🛫 SkyDesk — AI-Assisted Flight Booking Console

A terminal-based flight booking and management system written in **Python**, powered by **MySQL** for storage and **Google Gemini** for natural-language flight explanations.

---

## 📖 What This Is

SkyDesk mimics the backend of an airline's internal booking desk. Staff can manage flights, passengers, and reservations directly from a command-line dashboard — and instead of reading raw database fields off a screen, they can ask an AI assistant to translate a flight record into a plain-English summary a customer could actually understand.

The goal isn't to replace the database — it's to sit a language layer on top of it, so structured rows become readable sentences on demand.

---

## ✨ Core Capabilities

| Module | What it does |
|---|---|
| 🛩️ Flights | Create, list, look up, edit, and remove flight records |
| 🧳 Passengers | Register new passengers and browse existing ones |
| 📝 Bookings | Reserve a seat, list active reservations, cancel a booking |
| 💬 AI Explainer | Have Gemini describe a flight's details conversationally |
| 🔁 Seat Sync | Available seat counts update automatically as bookings change |

---

## 🧰 Built With

- **Python 3** — application logic
- **MySQL** — persistent storage (`mysql-connector-python`)
- **Gemini API** — natural-language generation (`google-genai`)
- **python-dotenv** — environment/config management

---

## 🗂️ Layout

```
skydesk/
├── main.py              # CLI dashboard — the app's entry point
├── booking_engine.py    # All DB queries + Gemini prompt orchestration
├── settings.py          # Reads GOOGLE_API_KEY and other env config
├── gemini_playground.py # Small standalone script for testing Gemini calls
└── .env                 # Local secrets (GOOGLE_API_KEY) — never committed
```

---

## 🏗️ Getting It Running

### 1. Grab the code
```bash
git clone https://github.com/yourname/skydesk.git
cd skydesk
```

### 2. Install what it needs
```bash
pip install mysql-connector-python python-dotenv google-genai
```

### 3. Stand up the database
Spin up a MySQL instance and run:

```sql
CREATE DATABASE skydesk;
USE skydesk;

CREATE TABLE flights (
    flight_id       INT AUTO_INCREMENT PRIMARY KEY,
    flight_no       VARCHAR(20),
    origin          VARCHAR(50),
    destination     VARCHAR(50),
    travel_date     DATE,
    departure_time  VARCHAR(10),
    arrival_time    VARCHAR(10),
    fare            DECIMAL(10,2),
    seats_available INT
);

CREATE TABLE passengers (
    passenger_id    INT AUTO_INCREMENT PRIMARY KEY,
    full_name       VARCHAR(100),
    email           VARCHAR(100),
    phone           VARCHAR(20)
);

CREATE TABLE bookings (
    booking_id      INT AUTO_INCREMENT PRIMARY KEY,
    passenger_id    INT,
    flight_id       INT,
    seat_no         VARCHAR(10),
    booked_on       DATE,
    FOREIGN KEY (passenger_id) REFERENCES passengers(passenger_id),
    FOREIGN KEY (flight_id) REFERENCES flights(flight_id)
);
```

### 4. Configure your Gemini key
Create a `.env` file at the project root:
```
GOOGLE_API_KEY=your_gemini_api_key_here
```
Free keys are available from [Google AI Studio](https://aistudio.google.com/).

### 5. Launch it
```bash
python main.py
```

---

## 🖱️ Using SkyDesk

Once launched, the dashboard looks like this:

```
============================================================
                    SKYDESK — FLIGHT DESK CLI
============================================================
 1. Add a Flight
 2. List Flights
 3. Look Up a Flight
 4. Edit a Flight
 5. Remove a Flight
 6. Register Passenger
 7. List Passengers
 8. Book a Seat
 9. List Bookings
10. Cancel a Booking
11. Explain a Flight (Gemini)
 0. Quit
============================================================
```

Choosing **11** and entering a flight number hands that flight's record to Gemini, which returns a friendly, plain-language summary — the kind you'd want a gate agent to read out loud, not a database dump.

---

## 🧩 How the AI Layer Works

1. You provide a flight number at the prompt.
2. `booking_engine.py` pulls the matching row from MySQL.
3. That row is wrapped in a prompt template that explicitly tells Gemini to stick to the given facts and not fabricate details.
4. Gemini's response streams back to the console as the final explanation.

Because the model only ever sees data pulled straight from the database, this is a lightweight retrieval-then-generation pattern rather than free-form chat — the AI narrates, it doesn't invent.

---

## 🧭 Roadmap

- [ ] Pull hardcoded DB credentials into `.env`
- [ ] Harden input validation across all CLI prompts
- [ ] Add a lightweight web UI (Flask or Streamlit) on top of the same engine
- [ ] Let Gemini handle fuzzy, natural-language searches ("anything nonstop to Delhi this weekend?")
- [ ] Add a test suite around the booking/cancellation logic
- [ ] Add multi-currency fare display

---

## 📄 License

MIT — do what you want with it, just don't blame us if a flight gets overbooked.
