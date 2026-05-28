# HU Events - Technical Documentation

**HU Events** is a modern, web-based university event management platform designed specifically for Hawassa University. It serves as a centralized hub for students, event organizers, and administrators to discover, manage, and oversee campus activities.

## Table of Contents
1. [Overview & Features](#overview--features)
2. [Technology Stack](#technology-stack)
3. [Project Structure](#project-structure)
4. [User Roles & Permissions](#user-roles--permissions)
5. [Frontend Architecture](#frontend-architecture)
6. [Backend Architecture](#backend-architecture)
7. [Database Schema Overview](#database-schema-overview)

---

## 1. Overview & Features

HU Events provides a complete ecosystem for university events, including:
- **Event Discovery:** Students can browse upcoming events, filter by categories (Tech, Sports, Education, etc.), and view detailed descriptions.
- **Registration System:** Students can register for events. The system tracks available seats, marking events as "Full" when capacity is reached.
- **Calendar Navigation:** An interactive calendar on the home page allows users to see which days have events and quickly jump to a list of events for that specific day.
- **Feedback System:** Students can leave a 1-to-5 star rating and comment on events they attended once the event is completed.
- **Notifications:** Users receive in-app notifications (e.g., successful registration alerts), complete with an unread badge counter in the navigation bar.
- **Role-Based Dashboards:** Dedicated dashboards tailored to the user's role (Admin, Organizer, Student).

---

## 2. Technology Stack

- **Frontend:** HTML5, CSS3 (Vanilla), Vanilla JavaScript (`script.js`). Phosphor Icons are used for iconography.
- **Backend:** PHP 8+ (Procedural / Functional approach via REST-like endpoints).
- **Database:** MySQL / MariaDB (connected via PHP Data Objects - PDO).

---

## 3. Project Structure

```text
c:\xampp\htdocs\event-update\
├── api/                    # REST-like JSON endpoints (auth, events, admin, etc.)
├── assets/                 # Static assets (images, icons, etc.)
├── config/                 # Configuration files (e.g., database connection params)
├── database/               # SQL scripts (schema.sql, seed.sql, migrations)
├── includes/               # Core PHP business logic (auth.php, db.php, events.php)
├── uploads/                # Directory for user-uploaded event images
├── *.html                  # Frontend Views (index, login, dashboards, events, etc.)
├── script.js               # Main Client-side logic engine (SPA-like routing & API handling)
└── styles.css              # Main stylesheet with custom CSS design tokens
```

---

## 4. User Roles & Permissions

The application supports three distinct user roles:

1. **Student (Customer):**
   - Can browse public events.
   - Can register and unregister for upcoming events.
   - Can submit ratings and feedback for past events they attended.
   - Dashboard: `customer-dashboard.html`

2. **Organizer:**
   - Can create, edit, and delete their own events.
   - Can view the list of students registered for their events.
   - Dashboard: `organizer-dashboard.html`

3. **Administrator (Admin):**
   - Can view overall platform statistics (total users, events, registrations).
   - Can manage user accounts (approve pending users, suspend accounts).
   - Can manage any event on the platform (mark as completed, cancel, etc.).
   - Dashboard: `admin-dashboard.html`

---

## 5. Frontend Architecture

The frontend uses Vanilla JavaScript to create a "Single Page Application" (SPA) feel across multiple HTML pages. 

- **State Management:** `script.js` uses an Immediately Invoked Function Expression (IIFE) to encapsulate state (e.g., `eventsCache`, `sessionUser`, `notificationsCache`).
- **Routing & Protection:** The `boot()` function runs on every page load. It uses `protectRoute()` to ensure that users cannot access dashboards that don't belong to their role. If an unauthenticated user tries to access a protected page, they are redirected to `login.html`.
- **API Communication:** A custom wrapper function `apiFetch()` handles all asynchronous communication with the backend PHP endpoints via the modern `fetch()` API.

---

## 6. Backend Architecture

The backend operates strictly as a JSON API, decoupling the server-side logic from the frontend presentation.

- **Endpoints (`/api/`):** Each feature has its own folder (e.g., `api/auth/login.php`, `api/events/create.php`). These scripts receive JSON payloads from `php://input`, process the request, and output a JSON response (`{"success": true, "data": ...}`).
- **Business Logic (`/includes/`):** To avoid code duplication, core logic is modularized in the `includes/` directory. For example, `db.php` returns a singleton PDO instance, and `session.php` handles JWT or standard session cookie validation.
- **Security:** SQL queries are executed using **PDO Prepared Statements** to prevent SQL injection.

---

## 7. Database Schema Overview

*(Located in `database/schema.sql`)*

- **`users` table:** Stores user credentials (hashed passwords), roles, and account status (active/pending/suspended).
- **`events` table:** Stores event details (title, description, date, time, location, capacity, image_path, status). Includes a foreign key `organizer_id` linking to the `users` table.
- **`categories` table:** Stores event categories (e.g., Technology, Sports) which are linked to events via `category_id`.
- **`registrations` table:** A junction table connecting `users` (students) and `events` to track who is attending what.
- **`notifications` table:** Stores system notifications tailored to individual users, along with an `is_read` boolean.
- **`feedback` table:** Stores star ratings (1-5) and text comments submitted by students for completed events.

---

*This documentation is generated to provide a comprehensive understanding of the HU Events application architecture and functionality.*
