# EventHub Ticket Booking System

## Overview
This project is a simple **EventHub Ticket Booking System** built for the ITUE301 Set F exam. It demonstrates a full‑stack JavaScript application using:
- **Frontend** – React with Vite, React Router, and Axios.
- **Backend** – Node.js, Express.js, MongoDB with Mongoose.
- **Authentication** – Basic token‑based mock authentication (suitable for a viva demonstration).

The code is intentionally kept easy to read and understand, with abundant comments explaining the flow of data, middleware, and React components.

---

## Folder Structure
```
/eventhub-booking
├─ frontend/                # React Vite app
│   ├─ src/
│   │   ├─ components/      # Reusable UI components (EventCard, NavBar)
│   │   ├─ pages/           # Page components (LoginPage, EventsPage, MyTicketsPage, OrganizerPanel)
│   │   ├─ context/         # AuthContext for authentication state
│   │   ├─ App.jsx
│   │   └─ main.jsx
│   ├─ index.html
│   ├─ package.json
│   └─ vite.config.js
│
├─ backend/                 # Express API server
│   ├─ models/               # Mongoose schemas (Attendee, Event, Ticket)
│   ├─ middleware/           # requestLogger, authGuard
│   ├─ routes/               # auth, events, tickets routes
│   ├─ seed.js               # Script to create sample events
│   ├─ server.js
│   ├─ .env.example
│   ├─ .gitignore
│   └─ package.json
│
├─ README.md                # This file – project documentation & viva guide
└─ .gitignore               # Root gitignore (ignores node_modules, .env, build output)
```

---

## Getting Started (Two‑Terminal Setup)
1. **Prerequisites** – Install Node.js (v20+ recommended) and ensure MongoDB Atlas/cluster is available.
2. **Clone / Copy** the project into `C:\Users\Administrator\Desktop\AWDF` (the workspace folder).
3. Open **two terminal windows**:
   - **Backend**:
     ```bash
     cd backend
     npm install
     cp .env.example .env   # Fill in your MongoDB connection string and PORT
     npm run dev            # Starts Express server on PORT (default 5000)
     ```
   - **Frontend**:
     ```bash
     cd frontend
     npm install
     npm run dev            # Starts Vite dev server (default http://localhost:5173)
     ```
4. Open the frontend URL in a browser. You should see the **Login** page.

---

## API Endpoints
| Method | URL | Description | Protected |
|--------|-----|-------------|-----------|
| POST   | `/api/v1/auth/login` | Mock login – returns a token and user info (role: `attendee` or `organizer`). | No |
| GET    | `/api/v1/events` | List all events. | No |
| POST   | `/api/v1/tickets` | Book tickets for an event. Decrements `ticketsAvailable`. | Yes |
| GET    | `/api/v1/tickets/my` | Get tickets belonging to the logged‑in attendee (populated). | Yes |
| PATCH  | `/api/v1/tickets/:id/status` | Update ticket status (`booked`, `cancelled`, `used`). | Yes |

---

## Running Tests (Manual)
- Use **Postman** or **curl** to hit the endpoints.
- Example login request (replace values):
  ```bash
  curl -X POST http://localhost:5000/api/v1/auth/login \
       -H "Content-Type: application/json" \
       -d '{"email":"john@example.com","password":"secret","role":"attendee"}'
  ```
- The response contains a `token` – copy it and add `Authorization: Bearer <token>` header for the protected routes.

---

## Viva Presentation Tips
- **Explain the folder layout** – separate concerns (frontend vs backend).
- **Highlight middleware** – `requestLogger` logs every request; `authGuard` validates the Bearer token.
- **Show Mongoose relationships** – `Ticket` references both `Attendee` and `Event`, and we use `populate()` to fetch full details.
- **Demonstrate React Router protection** – `ProtectedRoute` component checks `AuthContext` and redirects appropriately.
- **Discuss over‑booking prevention** – backend checks `ticketsAvailable` before creating a ticket and atomically decrements it.
- **Talk about lazy loading** – `OrganizerPanel` is loaded with `React.lazy` and wrapped in `Suspense`.
- **Show client‑side search** – `EventsPage` filters the already‑fetched events array without extra API calls.

---

## Future Enhancements (Optional)
- Replace the mock login with real password hashing (bcrypt) and JWT signing.
- Add pagination for events.
- Implement email notifications on booking.
- Deploy to Vercel (frontend) and Render/Heroku (backend).

---

*Happy coding and good luck with your viva!*
