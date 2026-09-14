# DriveMate Project Explained

This file explains how the system works, what each role does, the movement of the application, and the code structure behind it.

## 1. Project overview

DriveMate is a local driving school management system designed for:

- student lesson booking
- instructor schedule management
- admin oversight and analytics
- payments and lesson tracking
- notifications and reminders

The app is built using:

- React + TypeScript on the frontend
- Node.js + Express on the backend
- Prisma ORM with SQLite database
- JWT authentication
- Local demo data for students, instructors, and admin

## 2. What the student does

The student journey begins when they log in to the app.

### Student actions

1. Log in with their email and password
2. See their dashboard with:
   - progress percentage
   - track (Code 8 / Code 10)
   - lesson count
   - upcoming lessons
   - smart recommendation suggestions
   - reminder alerts
3. Select a lesson slot from the booking page
4. Book a lesson only if:
   - the slot is still available
   - they are not double-booking the same time
   - they have valid lesson credits from a paid package
5. Receive a booking confirmation notification
6. View future reminders for booked lessons
7. Track completion history and instructor notes
8. Access messages and quiz practice

### Student backend logic

The backend checks the request before booking a lesson:

- the user must be logged in
- the user must have the STUDENT role
- the slot must exist
- the slot must not already be booked
- the student cannot double-book the same date and time
- the student must have remaining credit from a paid package

This is handled in the Express server in the route:

- `POST /api/bookings/:slotId`

Important backend validation happens before Prisma creates a booking record.

### Student reminder flow

The app also includes smart scheduling and reminders:

- `GET /api/scheduling/suggestions` returns the best lesson times
- `GET /api/scheduling/reminders` creates or returns upcoming reminders

The logic reviews confirmed bookings and generates reminders for lessons scheduled soon.

## 3. What the instructor does

The instructor has a teaching-focused dashboard.

### Instructor actions

1. Log in with instructor credentials
2. View their own schedule
3. Create available lesson slots
4. See which students booked those slots
5. Update student progress and lesson notes
6. Communicate with students through messages
7. Review their own teaching performance metrics

### Instructor backend logic

The instructor role is protected by route guards.

Only users with `INSTRUCTOR` or `ADMIN` roles can interact with:

- `POST /api/slots`
- `GET /api/slots/my-schedule`
- `DELETE /api/slots/:slotId`
- `PATCH /api/progress/:studentId`

This is enforced using middleware that checks the user token and role before the endpoint continues.

### Instructor side of the system

The instructor updates student performance using the progress route. Those updates are stored in the `StudentProgress` table, and they can include:

- progress percentage
- rating
- lesson notes
- completed lessons

## 4. What the admin does

The admin is the system owner and has the highest authority.

### Admin actions

1. Log in as admin
2. View total users, bookings, and platform stats
3. Manage users and roles
4. Monitor payments
5. Review vehicles and documents
6. View overall analytics and system health
7. Control platform-level data

### Admin backend logic

Admin-only routes include:

- `GET /api/admin/users`
- `PATCH /api/admin/users/:userId`
- `DELETE /api/admin/users/:userId`
- `GET /api/admin/analytics`
- `GET /api/admin/payments`

These endpoints require the admin token and role check before returning data.

## 5. Flow of the whole app

The real movement of the app is:

1. User logs in
2. Server verifies credentials and creates a JWT token
3. Frontend stores the token in local storage
4. Protected routes read the token and user role
5. The app redirects users to their role-specific dashboards
6. Role-based actions are allowed only after backend validation
7. Data is read and written through Express API routes
8. Prisma updates the SQLite database
9. Notifications and reminders are generated automatically

## 6. Why the backend is important

The frontend shows buttons and pages, but the real rules live in the backend.

Examples:

- a student cannot book an already-booked slot
- an instructor cannot create duplicate availability entries
- an admin cannot be demoted by themselves
- a student cannot access another student’s progress
- only admin can manage users

These rules are enforced in the Express routes, which prevents fake clicks or manipulated front-end behavior.

## 7. Code structure

### Frontend files

- `frontend/src/App.tsx` – app routing and navigation
- `frontend/src/context/AuthContext.tsx` – login and auth state
- `frontend/src/lib/api.ts` – all Axios API calls
- `frontend/src/pages/StudentDashboard.tsx` – student dashboard
- `frontend/src/pages/InstructorDashboard.tsx` – instructor dashboard
- `frontend/src/pages/AdminDashboard.tsx` – admin dashboard
- `frontend/src/pages/BookingPage.tsx` – booking page
- `frontend/src/pages/PaymentsPage.tsx` – payment flow
- `frontend/src/pages/NotificationsPage.tsx` – notifications screen

### Backend files

- `backend/server.js` – all API logic, auth, booking routes, reminders, and scheduling
- `backend/prisma/schema.prisma` – database structure
- `backend/prisma/seed.js` – seed demo users and sample data

## 8. Technologies used

### Frontend

- React 18
- TypeScript
- React Router DOM
- Tailwind CSS
- Axios
- Lucide icons
- Recharts

### Backend

- Node.js
- Express.js
- Prisma ORM
- SQLite database
- JWT
- bcryptjs
- CORS

### Why they were chosen

- React makes building dashboards and pages easier
- TypeScript makes the code safer and cleaner
- Tailwind makes the interface modern and consistent
- Express makes API routes easy to create and manage
- Prisma simplifies database operations
- SQLite is perfect for a local machine demo system
- JWT makes login state secure and light
- bcryptjs keeps user passwords protected

## 9. Database explanation

The database is defined in `backend/prisma/schema.prisma` and contains the main models:

- `User` – accounts and roles
- `StudentProgress` – student performance and notes
- `AvailabilitySlot` – instructor availability
- `Booking` – lesson bookings
- `Payment` – package and payment records
- `Notification` – reminders and app notifications
- `Message` – internal communication
- `Feedback` – instructor evaluation feedback
- `QuizAttempt` – quiz result data
- `Vehicle` – driving vehicles and assignment

## 10. Smart scheduling feature

The app also includes a smart scheduling feature.

It calculates:

- student learning stage
- available future slots
- date and time preference
- best lesson fit for performance improvement

This makes the app more useful because the student is not just booking randomly; the system recommends good times and tracks upcoming lesson needs.

## 11. Demo credentials

### Admin
- Email: lesego@drivemate.co.za
- Password: Admin@123

### Instructor
- Email: sipho.khumalo@drivemate.co.za
- Password: Instructor@123

### Student
- Email: thando.zungu@example.co.za
- Password: Student@123

## 12. Summary

- Student: books lessons, pays, tracks progress, receives reminders
- Instructor: creates slots, tracks schedule, updates progress
- Admin: oversees users, analytics, payments, and management

The project is a real full-stack application made to be local, secure, practical, and easy to demo.
