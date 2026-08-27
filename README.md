# ✈️ Flight Reservation System with Gemini AI Assistant

A console-based **Flight Reservation System** built in Python, backed by a **MySQL** database, with an integrated **Gemini AI** feature that turns raw flight records into clear, customer-friendly explanations.

---

## 📌 Overview

This project simulates a real-world airline booking backend — manage flights, passengers, and reservations entirely from the terminal — while showcasing how a Generative AI model can be layered on top of structured data to improve the user experience.

Instead of showing a customer a raw database row, the app can ask Gemini to explain a flight's details in plain, natural language.

---

## 🚀 Features

| Category | Capabilities |
|---|---|
| ✈️ **Flight Management** | Add, view, search, update, and delete flights |
| 🧑‍🤝‍🧑 **Passenger Management** | Add and view passenger records |
| 🎟️ **Reservations** | Book a seat, view all reservations, cancel a reservation |
| 🤖 **AI Assistant** | Ask Gemini to explain a flight's details in simple language |
| 🔒 **Data Integrity** | Seat counts automatically adjust on booking/cancellation |

---

## 🛠️ Tech Stack

- **Language:** Python
- **Database:** MySQL (via `mysql-connector-python`)
- **AI Model:** Google Gemini (`google-genai` SDK)
- **Config Management:** `python-dotenv`

---

## 📂 Project Structure

```
gemini_chatbot_function/
├── main.py                 # Entry point — interactive CLI dashboard
├── flight_reservation.py   # Core business logic: DB operations + Gemini integration
├── config.py                # Loads the Gemini API key from environment variables
├── app.py                   # Standalone minimal Gemini Q&A script (prototype)
└── .env                      # Stores GOOGLE_API_KEY (not committed)
```

---

## ⚙️ Setup & Installation

### 1. Clone the repository
```bash
git clone https://github.com/cloudsony999/gemini_chatbot_function.git
cd gemini_chatbot_function
```

### 2. Install dependencies
```bash
pip install mysql-connector-python python-dotenv google-genai
```

### 3. Set up the MySQL database
Create a database named `flightdb` with the following tables:

```sql
CREATE DATABASE flightdb;
USE flightdb;

CREATE TABLE flights (
    flight_id INT AUTO_INCREMENT PRIMARY KEY,
    flight_no VARCHAR(20),
    source VARCHAR(50),
    destination VARCHAR(50),
    travel_date DATE,
    departure_time VARCHAR(10),
    arrival_time VARCHAR(10),
    price DECIMAL(10,2),
    seats INT
);

CREATE TABLE passengers (
    passenger_id INT AUTO_INCREMENT PRIMARY KEY,
    passenger_name VARCHAR(100),
    email VARCHAR(100),
    phone VARCHAR(20)
);

CREATE TABLE reservations (
    reservation_id INT AUTO_INCREMENT PRIMARY KEY,
    passenger_id INT,
    flight_id INT,
    seat_no VARCHAR(10),
    booking_date DATE,
    FOREIGN KEY (passenger_id) REFERENCES passengers(passenger_id),
    FOREIGN KEY (flight_id) REFERENCES flights(flight_id)
);
```



### 4. Add your Gemini API key
Create a `.env` file in the project root:
```
GOOGLE_API_KEY=your_gemini_api_key_here
```
Get a free key from [Google AI Studio](https://aistudio.google.com/).

### 5. Run the application
```bash
python main.py
```

---

## 🖥️ Usage

Once running, you'll see an interactive menu:

```
============================================================
       FLIGHT RESERVATION SYSTEM
============================================================
1. Add Flight
2. View Flights
3. Search Flight
4. Update Flight
5. Delete Flight
6. Add Passenger
7. View Passengers
8. Reserve Flight
9. View Reservations
10. Cancel Reservation
11. Ask Gemini About Flight
0. Exit
============================================================
```

Select option **11** and enter a flight number — Gemini will fetch that flight's details from MySQL and explain them in plain language for a customer.

---

## 🧠 How the AI Integration Works

1. The user enters a flight number.
2. `flight_reservation.py` queries MySQL for that flight's full record.
3. The record is formatted into a structured prompt instructing Gemini to explain the details **without inventing information**.
4. The prompt is sent to the Gemini API, and the generated explanation is printed to the console.

This keeps the AI grounded strictly in real database values — a simple but effective example of retrieval-then-generation.

---

## 🔮 Future Improvements

- [ ] Move DB credentials to `.env` instead of hardcoding them
- [ ] Add input validation and error handling for user inputs
- [ ] Build a web/GUI frontend (Flask or Streamlit)
- [ ] Extend Gemini integration to support natural-language flight search (e.g., "find me a flight to Delhi next week")
- [ ] Add unit tests for database operations

---
