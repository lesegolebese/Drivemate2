# DriveMate Driving School Management System

DriveMate is a local full-stack driving school management system built to help a driving school manage students, instructors, bookings, payments, progress, messaging, reminders, and admin reporting without needing a cloud platform or external database.

It runs completely on a single machine using a local SQLite database and Node.js backend, with a React + TypeScript frontend for the user experience.

## 1. Project purpose

The goal of the project is to manage a driving school in one system where:

- Students can view lessons, book driving sessions, pay for packages, track progress, and receive smart reminders.
- Instructors can create availability, manage lesson schedules, and review student progress and notes.
- Admins can oversee the platform, monitor users, handle payments, and view system analytics.

This is designed for a local mentor/demo environment, so all data is seeded with realistic sample users and can be run without internet or external services.

## 2. How the system works

### Student flow

A student starts by logging in, then sees a student dashboard showing:

- progress percentage
- license track
- lessons completed vs total
- upcoming lessons
- smart scheduling recommendations
- reminder count

From there, the student can:

1. Open the booking page.
2. View available instructor slots.
3. Select a lesson time that matches their schedule.
4. Book the lesson if they have lesson credits from a paid package.
5. Receive a booking confirmation notification.
6. Receive automatic lesson reminders for upcoming lessons.
7. View lesson history and progress notes.
8. Use the quiz feature to practice K53-related questions.

The real logic sits in the backend. The server validates whether the user is a student, checks whether the slot is free, prevents double-booking, and confirms whether the student has available lesson credits before creating the booking.

Example flow:

- Frontend sends a POST request to /api/bookings/:slotId
- Backend checks slot status, user role, and existing bookings
- Backend verifies payment package credits
- If valid, the slot is set as booked and a booking record is created
- A notification is created for the student
- The reminder generator checks the upcoming lesson and prepares reminder updates

### Instructor flow

An instructor logs in and sees their own dashboard with:

- their teaching schedule
- upcoming student bookings
- performance summary
- student progress information
- communication tools

From there, the instructor can:

1. Create lesson availability slots.
2. View their own schedule through /api/slots/my-schedule.
3. Check which students booked their lessons.
4. Update lesson progress and notes for students.
5. Send or view messages to students.

On the backend, instructor actions are protected by role-based authorization. Only users with the INSTRUCTOR or ADMIN role can access slot management and student progress updates.

Example flow:

- Frontend calls /api/slots
- Express verifies the JWT token and role
- Prisma checks for duplicate slot conflicts
- If no conflict exists, the slot is created
- The instructor can later view all their slots and booked student details

### Admin flow

An admin logs in and has access to the broadest controls in the application. The admin dashboard is used for:

- viewing platform analytics
- managing users and roles
- monitoring payments
- reviewing vehicles and documents
- checking general system data

Admin can:

1. Update user roles
2. Delete accounts when needed
3. View all users with pagination
4. Review financial summaries
5. Manage system-wide records

The backend only allows the ADMIN role to hit user management and analytics endpoints. This is enforced using middleware that checks req.user.role before the request proceeds.

Example flow:

- Frontend requests /api/admin/users
- JWT is checked
- Role middleware confirms ADMIN
- Prisma queries user records and returns paginated results

## 3. Code behind the logic

### Frontend structure

The frontend is built in React + TypeScript inside the `frontend/src` folder.

Main frontend files include:

- `frontend/src/App.tsx` – routing, navigation, protected routes, and role-based layout
- `frontend/src/context/AuthContext.tsx` – login state and session handling
- `frontend/src/lib/api.ts` – centralized Axios API calls
- `frontend/src/pages/StudentDashboard.tsx` – student dashboard
- `frontend/src/pages/InstructorDashboard.tsx` – instructor dashboard
- `frontend/src/pages/AdminDashboard.tsx` – admin dashboard
- `frontend/src/pages/BookingPage.tsx` – lesson booking flow
- `frontend/src/pages/PaymentsPage.tsx` – package purchasing and payments
- `frontend/src/pages/NotificationsPage.tsx` – reminder and system notifications

The frontend uses React Router to redirect users based on auth state and role. Protected routes are wrapped in a `ProtectedRoute` component that prevents unauthorised users from entering pages they should not access.

### Backend structure

The backend is a Node.js Express application in `backend/server.js`.

Core backend logic includes:

- JWT authentication
- role-based authorization middleware
- Prisma database queries
- holiday/scheduling recommendation logic
- reminder generation logic
- booking validation rules

Key patterns used in the server:

- `authenticateToken` verifies the JWT before allowing a request
- `authorize(['ROLE'])` ensures only the correct role can access the route
- Prisma transactions are used to book a slot safely
- duplicate scheduling checks prevent invalid slot creation
- payment credits are checked before a booking is accepted

### Smart scheduling and reminder logic

This part is important because it shows how the app becomes more than a booking system.

The backend has two main helper functions:

- `getBestSchedulingSuggestions(studentId)`
- `generateLessonReminderNotifications(studentId)`

The scheduling engine:

- reads the student's progress
- finds open slots in the near future
- scores each slot based on proximity and learning priority
- recommends the best lesson times for the student

The reminder engine:

- finds confirmed bookings within the next 7 days
- checks if a reminder already exists
- creates a notification if one is needed
- sends the reminder to the student and stores it in the database

This logic is exposed through:

- `GET /api/scheduling/suggestions`
- `GET /api/scheduling/reminders`

## 4. Technologies used

### Frontend

- React 18
- TypeScript
- React Router DOM
- Tailwind CSS
- Axios
- Lucide React icons
- Recharts for analytics visualizations

Why these were chosen:

- React gives a fast component-based UI
- TypeScript reduces runtime errors and makes the code cleaner
- Tailwind makes the UI fast to build and consistent
- Axios simplifies API requests from the frontend
- Recharts helps with admin analytics cards

### Backend

- Node.js
- Express.js
- Prisma ORM
- SQLite
- JWT
- bcryptjs
- CORS

Why these were chosen:

- Node.js is fast for API development
- Express is simple and effective for REST endpoints
- Prisma makes database access safer and easier to maintain
- SQLite is ideal for local hosting and demo environments
- JWT lets the app authenticate users statelessly
- bcryptjs securely hashes passwords so they are not stored in plain text

### Database model

The database is defined in `backend/prisma/schema.prisma` and includes tables such as:

- `User`
- `StudentProgress`
- `AvailabilitySlot`
- `Booking`
- `Payment`
- `Notification`
- `Message`
- `Feedback`
- `QuizAttempt`
- `Vehicle`
- `Document`
- `Attendance`

This structure allows the app to keep all core business data together in one local SQLite database.

## 5. Real application flow from login to lesson booking

The core movement of the app is:

1. User logs in
2. JWT token is created and returned
3. Frontend stores token and loads user data
4. Route protection checks role and redirects unauthorized users
5. User lands on their role-specific dashboard
6. Student books from available slots
7. Backend validates slot data and credits
8. Booking is stored in the database
9. Confirmation notification is created
10. Reminder system checks future bookings and creates reminder notifications
11. Admin views analytics and payment records
12. Instructor updates progress and schedule notes

This flow keeps the system logical and ensures that the frontend does not have to be trusted for security decisions. Important checks happen on the backend.

## 6. Why this project is efficient and reliable

This project was designed to be:

- local-first
- easy to run on one device
- role-based
- realistic for a school operations demo
- consistent in logic and UI flow

It is efficient because:

- the same local SQLite database powers everything
- the backend acts as the central source of truth
- role security is enforced in the server, not only in the UI
- the reminder and scheduling logic helps students take action proactively

## 7. Demo credentials

### Admin
- Email: lesego@drivemate.co.za
- Password: Admin@123

### Instructor
- Email: sipho.khumalo@drivemate.co.za
- Password: Instructor@123

### Student
- Email: thando.zungu@example.co.za
- Password: Student@123

## 8. How to run locally

Install dependencies:

```bash
npm run setup
```

Push Prisma schema to SQLite and seed demo data:

```bash
npm run db:setup
```

Start the app:

```bash
npm run dev
```

Then open:

- Frontend: http://localhost:3000
- Backend: http://localhost:5000

## 9. Project structure

```bash
Drivemate2/
├── backend/
│   ├── prisma/
│   │   ├── schema.prisma
│   │   └── seed.js
│   ├── server.js
│   ├── package.json
│   └── dev.db
├── frontend/
│   ├── src/
│   └── package.json
├── package.json
├── README.md
└── node_modules/
```

## 10. Conclusion

DriveMate is a complete local driving school management system built to mimic real operations in a simple, understandable, and practical way. It combines backend logic, database design, role-based access, and frontend UI into one working application.

The student flow is focused on booking and learning progress, the instructor flow is centered on schedule and student coaching, and the admin flow is focused on system control and oversight.

This project is a strong full-stack example of how a real business workflow can be implemented in a simple local environment without depending on cloud services.

## 11. Credits

Project developed for local driving school management and demo purposes.
