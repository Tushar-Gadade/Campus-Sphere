# CampusSphere - Product Requirements Document (PRD)

## 1. Executive Summary & Vision
**CampusSphere** is a campus peer marketplace and services exchange platform designed for university students. Students can buy and sell second-hand goods (books, electronics, dorm essentials) and offer or book campus-based services (tutoring, resume review, instrument lessons). It combines a real-time chat system, a booking and scheduling engine, and a peer review system to create a trusted, campus-native exchange community.

---

## 2. Target Audience

| Persona | Goals | Problems Solved |
| :--- | :--- | :--- |
| **Student (Buyer / Requester)** | Find affordable second-hand goods; book peer tutoring or skill-based services. | Expensive bookshops, unreliable Facebook groups, no safe campus-verified platform. |
| **Student (Seller / Provider)** | Offload used textbooks, equipment, or earn money offering skills and time. | No structured way to advertise or manage availability, bookings, or payment expectations. |
| **Campus Administrator** | Moderate content, suspend bad actors, view platform activity statistics. | No visibility or control over peer commerce happening in unofficial channels. |

---

## 3. Core Features

### 3.1 Authentication & User Profiles
- Register and login via email and password (campus-native; no OAuth required in v1).
- JWT-based stateless session; roles: `STUDENT` and `ADMIN`.
- Profile: name, department, year, bio, skill tags, avatar (Cloudinary upload), average rating from reviews.
- Owner-only profile editing.

### 3.2 Marketplace (Listings)
- Students post listings: title, description, price, category, condition, up to 4 images.
- Categories: Books, Electronics, Tutoring, Instruments, Clothing, Sports, Stationery, Furniture, Others.
- Listing statuses: `ACTIVE`, `SOLD`, `PAUSED`.
- Browse with search, category filter, price range, and sort; state persisted in URL.
- Interest count tracked when "Contact seller" is triggered (used by Discover feed).
- Sellers manage their own listings (edit, delete, status toggle).

### 3.3 Services Board
- Students offer time-based services (tutoring, skills, help) with availability slots (date + start + end).
- Slots locked atomically on booking to prevent double-booking.
- Filter by category and free/paid toggle.
- Service detail shows only unbooked upcoming slots.

### 3.4 Booking System
- One-click RSVP to a specific slot with an optional message.
- Transactional slot lock: prevents race conditions on simultaneous booking attempts.
- Provider can Accept, Reject, or mark Complete. Requester can Cancel while Pending.
- On reject or cancel, slot is freed automatically.
- Bookings list with Sent / Received tabs.

### 3.5 Real-Time Chat
- One-on-one conversations initiated via "Contact seller" or "Contact provider" buttons.
- Messages saved to database and delivered live via Socket.io.
- Persistent history loaded via REST on open.
- Typing indicators.

### 3.6 Notifications
- In-app notification system triggered by: new booking request, booking status change, new chat message, new review.
- Notification bell in Navbar with unread count (polling every 30 s).
- Notifications page with read/unread state.

### 3.7 Reviews
- Reviewable only after a booking reaches `COMPLETED` status.
- One review per booking, submitted by the requester only.
- Star rating (1–5) and text comment.
- Provider's `average_rating` recalculated in a Prisma transaction on each new review.
- Reviews displayed on profile pages and service cards.

### 3.8 Home / Discover Feed
- Trending Listings (by `interest_count`), Suggested Services (by `booking_count`), Newest Listings, Newest Services.
- Category chip row for quick navigation.
- Skeleton loaders for all data-driven sections.

### 3.9 Admin Dashboard
- Platform stats: total users, listings, services, bookings.
- User management: view all users, suspend / unsuspend (suspended users cannot log in).
- Content moderation: remove listings and services.
- Every admin action logged to `AdminAction` table.

---

## 4. Non-Functional Requirements

- **Performance**: API responses under 300 ms for standard queries; Vite build outputs optimized chunks.
- **Security**:
  - JWT in `localStorage` with `Authorization` header on all requests.
  - Passwords hashed with bcrypt (rounds ≥ 10).
  - Helmet headers, CORS restricted to `CLIENT_URL`, rate-limited auth endpoints.
  - Cloudinary API secret never exposed to the browser.
- **Availability**: Free-tier Render + Neon + Vercel deployment. UptimeRobot pings `/health` every 5 min. README notes expected cold-start delay.
- **Responsiveness**: Mobile-first Tailwind layout; all pages tested at 375px viewport.
- **Error Handling**: Uniform `{success, data/error}` API envelope; no raw stack traces in production; toast notifications for all mutations.
- **Testing**: Auth and booking flows covered by automated tests; tests run in CI before every merge to `main`.

---

## 5. Out of Scope (v1)
- Payments or in-app transactions (price is informational only).
- Push / email notifications.
- Group chats or public forums.
- OAuth / SSO login.
- Mobile native app.
