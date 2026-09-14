# DriveMate Driving School Management System
## A Beginner-Friendly Guide to Understanding the Project

## What is DriveMate?

DriveMate is a web application that helps driving schools manage their daily operations. Think of it as a digital system that brings together students, driving instructors, and school administrators in one place. Instead of using paper records, phone calls, and spreadsheets, everything happens through this web application.

### What Can Users Do?

**Students can:**
- Book driving lessons online
- See their learning progress
- Take practice quizzes for their license test
- Pay for lessons
- Message their instructors
- Upload required documents (like ID or learner's license)

**Instructors can:**
- Set their available time slots
- See which students they're teaching
- Mark attendance for lessons
- Give feedback to students
- Message students

**Administrators can:**
- Manage all users in the system
- Approve or reject uploaded documents
- Manage the fleet of vehicles
- See payments and financial reports
- Have full control over the system

---

## How the Project is Built (The Technology Stack)

### The Two Main Parts

DriveMate is built with two separate parts that work together:

1. **Backend** - The "brain" that runs on the server
2. **Frontend** - The "face" that users see in their browser

---

## Part 1: The Backend (Server-Side)

### What is the Backend?

The backend is the part of the application that runs on a server computer. Users never see this part directly, but it does all the important work in the background. Think of it like the kitchen in a restaurant - customers don't see it, but that's where all the food is prepared.

### Where is it located?

**Main file:** `backend/server.js` (this is a big file with over 1500 lines of code)

### Technologies Used in the Backend

**Node.js** - This is the foundation. It allows JavaScript to run on a server (normally JavaScript only runs in web browsers). Think of it as the engine that powers the whole backend.

**Express.js** - This is a framework built on top of Node.js. It makes it easier to create web servers and handle requests. Think of it as a toolkit that helps build the server faster.

**Prisma ORM** - This is a tool that helps the backend talk to the database. Instead of writing complex database queries, we use simple JavaScript commands. Think of it as a translator between our code and the database.

**SQLite** - This is the database where all the data is stored. It's a simple file-based database that's perfect for local development. Think of it as a digital filing cabinet that stores all user information, bookings, payments, etc.

**JWT (JSON Web Tokens)** - This is how we handle security. When a user logs in, we give them a special token (like a digital ID card) that proves who they are. Think of it like a wristband at an event that shows you're allowed to enter certain areas.

**Passport.js** - This is a library that helps with authentication (checking if users are who they say they are). It handles the login process.

**bcryptjs** - This is used for password security. It turns passwords into scrambled code so even if someone gets into the database, they can't read the passwords. Think of it like a secret code that only the system understands.

**CORS** - This stands for Cross-Origin Resource Sharing. It allows the frontend (which might be on a different website) to talk to the backend safely. Think of it like a security guard that checks if requests are allowed.

### What Does the Backend Do?

The backend does these main things:

1. **Receives requests** from the frontend (like "show me available lessons" or "log this user in")
2. **Talks to the database** to get or save information
3. **Processes the data** (like checking if a password is correct, or calculating quiz scores)
4. **Sends responses** back to the frontend (like "here are the available lessons" or "login successful")

### API Endpoints (The Backend's "Menu")

The backend has different "endpoints" - these are like different doors that the frontend can knock on to get specific things. Here are the main ones:

**Authentication (Login/Register):**
- `POST /api/auth/register` - Create a new user account
- `POST /api/auth/login` - Log in an existing user
- `GET /api/auth/me` - Get information about the currently logged-in user
- `POST /api/auth/refresh` - Get a new access token when the old one expires

**User Management (for Admins):**
- `GET /api/admin/users` - See all users in the system
- `PATCH /api/admin/users/:id` - Change a user's role or information
- `DELETE /api/admin/users/:id` - Remove a user from the system

**Availability Slots (for Instructors):**
- `POST /api/slots` - Instructor creates a time slot when they're available
- `GET /api/slots/available` - Students see what time slots are available
- `GET /api/slots/my-schedule` - Instructor sees their own schedule
- `DELETE /api/slots/:id` - Instructor cancels a time slot

**Booking System:**
- `POST /api/bookings/:slotId` - Student books a lesson
- `GET /api/bookings/my-lessons` - Student sees their booked lessons
- `POST /api/bookings/:id/cancel` - Student cancels a booking

**Progress Tracking:**
- `GET /api/progress/:studentId` - See how far a student has progressed
- `PATCH /api/progress/:studentId` - Update a student's progress
- `GET /api/lessons/history` - See past lessons
- `GET /api/lessons/credits` - See how many paid lessons a student has left

**Payment Management:**
- `GET /api/payments` - See payment history
- `POST /api/payments` - Process a payment
- `PATCH /api/payments/:id` - Update payment status

**Messaging System:**
- `GET /api/messages/threads` - See all conversations
- `GET /api/messages/:peerId` - See messages with a specific person
- `POST /api/messages` - Send a message
- `PATCH /api/messages/:id/read` - Mark a message as read

**Quiz System:**
- `GET /api/quiz/questions` - Get quiz questions
- `POST /api/quiz/attempt` - Submit quiz answers

**Notification System:**
- `GET /api/notifications` - See user notifications
- `PATCH /api/notifications/:id/read` - Mark notification as read

**Document Management:**
- `POST /api/documents/upload` - Upload a document
- `GET /api/documents` - See uploaded documents
- `PATCH /api/documents/:id/review` - Admin reviews a document

**Vehicle Management:**
- `GET /api/vehicles` - See all vehicles
- `POST /api/vehicles` - Add a new vehicle
- `PATCH /api/vehicles/:id` - Update vehicle information
- `DELETE /api/vehicles/:id` - Remove a vehicle

**Attendance Tracking:**
- `POST /api/attendance/check-in` - Mark attendance for a lesson
- `GET /api/attendance` - See attendance records

### Smart Scheduling Algorithm

**Location:** `backend/server.js` lines 74-120

This is a special feature I built that helps students find the best lesson slots. It uses a scoring system:

- Every slot starts with 100 points
- Slots that are further away lose points (8 points per day)
- Morning slots (8-9 AM) get bonus points (+8)
- Afternoon slots (2-3 PM) get smaller bonus points (+5)
- Students who are beginners (less than 50% progress) get priority (+10 points)

This way, the system recommends the best slots for each student's situation.

---

## Part 2: The Database (Where Data Lives)

### What is the Database?

The database is where all the information is stored permanently. Think of it like a giant Excel file with many connected tables. Every user, booking, payment, and message is stored here.

### Where is it located?

**Schema file:** `backend/prisma/schema.prisma` (this defines what the database looks like)
**Database file:** `backend/prisma/dev.db` (this is the actual database file)

### What's in the Database?

The database has 12 main "models" (think of these as different types of data tables):

**1. User Model** - Stores information about every person in the system
   - Their name, email, password (scrambled for security), and role (student/instructor/admin)
   - This is the central model that connects to almost everything else

**2. StudentProgress Model** - Tracks how a student is doing
   - Which license code they're learning (Code 8 or Code 10)
   - How much progress they've made (percentage)
   - Their rating from instructors
   - How many lessons they've completed

**3. AvailabilitySlot Model** - When instructors are available
   - Which instructor
   - What date and time
   - Which vehicle
   - Whether it's already booked or still available

**4. Booking Model** - When students book lessons
   - Which student booked it
   - Which time slot they booked
   - The status (confirmed, cancelled, or completed)

**5. Payment Model** - Financial transactions
   - Which student paid
   - How much they paid
   - What package they bought (single lesson, starter pack, etc.)
   - Payment status (pending, paid, failed)

**6. Message Model** - Messages between users
   - Who sent it
   - Who received it
   - The message content
   - Whether it's been read

**7. Feedback Model** - Instructor feedback on students
   - Which student
   - Which instructor gave the feedback
   - The rating (1-5 stars)
   - Written notes about the lesson

**8. QuizAttempt Model** - Quiz results
   - Which user took the quiz
   - Which license code (8 or 10)
   - Their score
   - Whether they passed or failed
   - Their answers (stored as JSON)

**9. Notification Model** - Notifications for users
   - Which user
   - Type of notification (booking confirmed, lesson reminder, etc.)
   - The message
   - Whether it's been read

**10. RefreshToken Model** - For keeping users logged in
   - The token (like a digital key)
   - Which user it belongs to
   - When it expires

**11. Vehicle Model** - Information about driving school vehicles
   - Make, model, year
   - License plate
   - Type (manual or automatic)
   - Which license code it's for (8, 10, or 14)
   - Status (available, in use, maintenance, out of service)

**12. Document Model** - Documents uploaded by students
   - Which student uploaded it
   - Type of document (ID, learner's license, etc.)
   - File information
   - Status (pending, approved, rejected)

### How Data Connects

The database models are connected to each other. For example:
- A User can have many Bookings
- A Booking belongs to one AvailabilitySlot
- An AvailabilitySlot belongs to one Vehicle
- A User can have many Payments
- A User can send and receive many Messages

These connections ensure that all the data stays consistent. If you delete a user, all their bookings, messages, and payments are also deleted automatically (this is called "cascade delete").

---

## Part 3: The Frontend (What Users See)

### What is the Frontend?

The frontend is the part of the application that users actually see and interact with in their web browser. It's the "face" of the application - the buttons, forms, pages, and everything visual.

### Where is it located?

**Main folder:** `frontend/src/`

### Technologies Used in the Frontend

**React** - This is a JavaScript library for building user interfaces. It makes it easy to create interactive pages that update without reloading the whole page. Think of it like building with LEGO blocks - each part is a separate component that fits together.

**TypeScript** - This is like JavaScript but with superpowers. It adds "types" which help catch errors before the code even runs. Think of it like spell-check for code - it helps prevent mistakes.

**TailwindCSS** - This is a CSS framework that makes styling much faster. Instead of writing custom CSS for everything, we use pre-made classes. Think of it like having a box of pre-cut furniture pieces instead of cutting wood yourself.

**React Router DOM** - This handles navigation in the application. It lets users move between different pages (like from login to dashboard) without the page reloading. Think of it like the navigation system in a building.

**Axios** - This is a tool for making HTTP requests to the backend. It's how the frontend talks to the backend. Think of it like a telephone that lets the frontend call the backend.

**Lucide React** - This provides icons for the interface (like home icons, user icons, etc.). Think of it like a library of emoji-style icons.

**Recharts** - This is for creating charts and graphs (like progress charts). Think of it like a tool for drawing visual data.

**date-fns** - This helps with working with dates and times. Think of it like a calendar calculator.

### Frontend Structure

The frontend is organized into these main folders:

**context/** - This holds "Context Providers" which are like global storage for information that needs to be accessed from anywhere in the app
- `AuthContext.tsx` - Manages user login state (who is logged in, what's their role)
- `ThemeContext.tsx` - Manages the visual theme (light or dark mode)

**lib/** - This holds utility libraries
- `api.ts` - This is a central file that contains all the functions for talking to the backend. Instead of writing API calls in every component, they're all organized here.

**pages/** - This holds all the different pages of the application
- `LandingPage.tsx` - The public homepage that anyone can see
- `LoginPage.tsx` - Where users log in
- `StudentDashboard.tsx` - The main page for students
- `InstructorDashboard.tsx` - The main page for instructors
- `AdminDashboard.tsx` - The main page for administrators
- `BookingPage.tsx` - Where students book lessons
- `PaymentsPage.tsx` - Where students see and make payments
- `ProgressPage.tsx` - Where students see their learning progress
- `QuizPage.tsx` - Where students take practice quizzes
- `MessagesPage.tsx` - The messaging system
- `NotificationsPage.tsx` - Where users see their notifications
- `VehiclesPage.tsx` - Where admins manage vehicles
- `DocumentsPage.tsx` - Where students upload documents
- `AttendancePage.tsx` - Where instructors mark attendance
- `ProfilePage.tsx` - Where users manage their profile

**types/** - This holds TypeScript type definitions
- `index.ts` - Defines what kind of data each piece of information should be (like "a User has a name which is a string, an email which is a string, etc.")

**App.tsx** - This is the main file that ties everything together. It sets up the routing (which page shows for which URL) and wraps the whole app in the context providers.

### How the Frontend Works

1. **User visits a page** (like the login page)
2. **React renders the component** for that page (shows the login form)
3. **User interacts** (types their email and password, clicks login)
4. **Frontend calls the API** (using the functions in `api.ts`)
5. **Backend processes the request** and sends back a response
6. **Frontend updates the UI** based on the response (shows success message or error)
7. **User is redirected** to the appropriate page (like the dashboard)

### Authentication Context

**Location:** `frontend/src/context/AuthContext.tsx`

This is a very important part of the frontend. It manages the authentication state - basically, it keeps track of:
- Is someone logged in?
- Who is logged in?
- What's their access token (their digital ID card)?
- What's their role (student, instructor, or admin)?

Any component in the app can access this information using a special hook called `useAuth()`. This way, the app can show different things to different users (like showing the admin dashboard only to admins).

### API Client

**Location:** `frontend/src/lib/api.ts`

This file is like a phone book for the backend. It contains organized functions for every API endpoint:

- `authAPI` - Functions for login, register, getting user info
- `usersAPI` - Functions for managing users
- `slotsAPI` - Functions for availability slots
- `bookingsAPI` - Functions for bookings
- `progressAPI` - Functions for progress tracking
- `paymentsAPI` - Functions for payments
- `messagesAPI` - Functions for messaging
- `notificationsAPI` - Functions for notifications
- `quizAPI` - Functions for quizzes
- `documentsAPI` - Functions for documents
- `vehiclesAPI` - Functions for vehicles
- `attendanceAPI` - Functions for attendance

The best part is that this file automatically adds the user's access token to every request, so we don't have to manually add it every time.

---

## How Authentication (Login) Works

This is a crucial part of the system - how we make sure only the right people can access the right things.

### Step 1: Registration (Creating an Account)

1. User fills out the registration form (name, email, password)
2. Frontend sends this data to the backend via `POST /api/auth/register`
3. Backend uses bcrypt to scramble the password (so it's not stored as plain text)
4. Backend creates a new User record in the database
5. Backend generates two tokens:
   - Access token (short-lived, like 1 hour) - for immediate use
   - Refresh token (long-lived, like 7 days) - for getting new access tokens
6. Backend sends the user data and tokens back to the frontend
7. Frontend stores the access token in the browser's localStorage (like a digital wallet)
8. Frontend updates the AuthContext to show the user is logged in

### Step 2: Login (Signing In)

1. User enters their email and password
2. Frontend sends this to the backend via `POST /api/auth/login`
3. Passport.js (the authentication library) checks if the email exists in the database
4. Backend uses bcrypt to compare the password the user entered with the scrambled password in the database
5. If they match, backend generates new access and refresh tokens
6. Backend sends the user data and tokens back to the frontend
7. Frontend stores the tokens and updates the AuthContext

### Step 3: Using the App (Access Token)

1. When the user navigates to any page, the frontend checks if they have an access token
2. For every API request, the frontend automatically includes the access token in the request header (like showing an ID card)
3. Backend validates the token to make sure it's genuine and not expired
4. If valid, backend processes the request
5. If invalid or expired, backend rejects the request

### Step 4: Token Refresh (Getting a New Access Token)

1. When the access token expires (after 1 hour), the frontend uses the refresh token
2. Frontend calls `POST /api/auth/refresh` with the refresh token
3. Backend checks if the refresh token exists in the database and isn't expired
4. If valid, backend generates a new access token
5. Backend sends the new access token back to the frontend
6. Frontend replaces the old access token with the new one
7. User stays logged in without having to log in again

### Step 5: Logout (Signing Out)

1. User clicks the logout button
2. Frontend removes the tokens from localStorage
3. Frontend updates the AuthContext to show no user is logged in
4. Frontend redirects to the login page

---

## Role-Based Access Control (Who Can Do What)

The system has three types of users, and each can do different things:

### STUDENT
- Can book lessons
- Can see their own progress
- Can take quizzes
- Can make payments
- Can send messages to instructors
- Can upload documents
- Can see their own attendance

### INSTRUCTOR
- Can create availability slots (when they're free to teach)
- Can see their schedule
- Can mark attendance for their students
- Can give feedback to students
- Can message students
- Can see vehicle assignments

### ADMIN
- Can do everything students and instructors can do
- Can manage all users (create, edit, delete, change roles)
- Can approve or reject documents
- Can manage the vehicle fleet
- Can see all payments and financial reports
- Has full control over the system

### How This Works in the Code

**In the Backend:**
- There's a function called `authorize()` that checks if a user has the right role
- This function is used as "middleware" - it runs before the main endpoint code
- If the user doesn't have the right role, they get a "403 Forbidden" error

**In the Frontend:**
- There's a component called `ProtectedRoute` that wraps sensitive pages
- It checks if the user is logged in and has the right role
- If not, it redirects them to the login page or their appropriate dashboard

---

## Example: How Booking a Lesson Works

Let's walk through what happens when a student books a lesson, step by step:

1. **Student logs in** and goes to the Booking Page
2. **Frontend calls** `slotsAPI.getAvailable()` which sends a request to `GET /api/slots/available`
3. **Backend queries** the database for all AvailabilitySlot records where `isBooked = false`
4. **Backend sends back** the available slots to the frontend
5. **Frontend displays** the slots in a nice calendar format
6. **The smart scheduling algorithm** (in the backend) also scores each slot and recommends the best ones
7. **Student selects** a slot and clicks "Book"
8. **Frontend calls** `bookingsAPI.book(slotId)` which sends a request to `POST /api/bookings/:slotId`
9. **Backend creates** a new Booking record in the database
10. **Backend updates** the AvailabilitySlot to set `isBooked = true`
11. **Backend creates** a Notification for the instructor to let them know they have a new booking
12. **Backend sends back** the booking confirmation to the frontend
13. **Frontend shows** a success message and redirects to the student's dashboard
14. **Student can now see** the booked lesson in their dashboard

This whole process happens in just a few seconds!

---

## Example: How Taking a Quiz Works

1. **Student goes to** the Quiz Page
2. **Student selects** which license code they want to practice (Code 8 or Code 10)
3. **Frontend calls** the quiz API to get questions for that code
4. **Backend sends back** the questions
5. **Frontend displays** the questions one by one
6. **Student answers** each question
7. **Frontend calculates** the score locally (for immediate feedback)
8. **Student submits** the quiz
9. **Frontend calls** `POST /api/quiz/attempt` with the answers
10. **Backend creates** a QuizAttempt record in the database
11. **Backend determines** if the student passed (based on percentage)
12. **Backend sends back** the results
13. **Frontend displays** whether they passed or failed with their score

---

## Security Features

### Password Security
- Passwords are never stored as plain text
- They're scrambled using bcrypt (a one-way encryption)
- Even if someone hacks the database, they can't read the passwords

### Token Security
- Access tokens expire quickly (1 hour) so if stolen, they're only useful for a short time
- Refresh tokens are stored in the database, not just in the browser
- Tokens can be invalidated (like cancelling a lost credit card)

### API Security
- CORS ensures only the frontend can make requests to the backend
- Input validation prevents malicious data from being processed
- SQL injection prevention (Prisma ORM handles this automatically)

### Role-Based Security
- Every sensitive endpoint checks the user's role
- Frontend also checks roles before showing certain pages
- Double protection ensures users can't access things they shouldn't

---

## How to Run the Project

### What You Need
- Node.js (version 18 or higher) - this runs the JavaScript code
- npm (comes with Node.js) - this manages the project's dependencies

### Step-by-Step Setup

1. **Open a terminal/command prompt**
2. **Navigate to the project folder:**
   ```bash
   cd Drivemate2/Drivemate2
   ```

3. **Install all dependencies:**
   ```bash
   npm run setup
   ```
   This installs everything needed for both the backend and frontend

4. **Set up the database:**
   ```bash
   npm run db:setup
   ```
   This creates the database and adds some test data

5. **Run the application:**
   ```bash
   npm run dev
   ```
   This starts both the backend and frontend

6. **Open your browser and go to:**
   ```
   http://localhost:5000
   ```

### Test Accounts

The system comes with pre-made accounts for testing:

**Admin:**
- Email: admin@drivemate.com
- Password: admin123

**Instructor:**
- Email: instructor@drivemate.com
- Password: instructor123

**Student:**
- Email: student@drivemate.com
- Password: student123

---

## Project Structure Overview

```
Drivemate2/
├── backend/                    # The server-side code
│   ├── config/
│   │   └── passport.js        # Handles login/authentication
│   ├── prisma/
│   │   ├── schema.prisma     # Database structure definition
│   │   ├── seed.js           # Script to add test data
│   │   └── dev.db            # The actual database file
│   ├── server.js             # The main server file (1500+ lines)
│   ├── .env                  # Secret configuration (passwords, etc.)
│   └── package.json          # Backend dependencies
├── frontend/                   # The user interface
│   ├── src/
│   │   ├── context/          # Global state management
│   │   │   ├── AuthContext.tsx    # Manages login state
│   │   │   └── ThemeContext.tsx   # Manages light/dark mode
│   │   ├── lib/              # Utility libraries
│   │   │   └── api.ts        # Functions to talk to backend
│   │   ├── pages/            # All the different pages
│   │   │   ├── LandingPage.tsx
│   │   │   ├── LoginPage.tsx
│   │   │   ├── StudentDashboard.tsx
│   │   │   ├── InstructorDashboard.tsx
│   │   │   ├── AdminDashboard.tsx
│   │   │   ├── BookingPage.tsx
│   │   │   ├── PaymentsPage.tsx
│   │   │   ├── ProgressPage.tsx
│   │   │   ├── QuizPage.tsx
│   │   │   ├── MessagesPage.tsx
│   │   │   ├── NotificationsPage.tsx
│   │   │   ├── VehiclesPage.tsx
│   │   │   ├── DocumentsPage.tsx
│   │   │   ├── AttendancePage.tsx
│   │   │   └── ProfilePage.tsx
│   │   ├── types/            # TypeScript type definitions
│   │   │   └── index.ts
│   │   ├── App.tsx           # Main app component with routing
│   │   ├── index.css         # Global styles
│   │   └── index.js          # Entry point
│   ├── public/               # Static files (images, etc.)
│   ├── tailwind.config.js    # TailwindCSS configuration
│   ├── tsconfig.json         # TypeScript configuration
│   └── package.json          # Frontend dependencies
├── package.json              # Root package with scripts
└── README_PRESENTATION.md    # This file
```

---

## Key Takeaways for Your Presentation

### What to Emphasize

1. **The System is Split in Two**
   - Backend (server) does the heavy lifting
   - Frontend (browser) is what users see
   - They communicate through API endpoints

2. **The Database is the Heart**
   - All data lives in the database
   - 12 interconnected models store everything
   - Prisma ORM makes database operations easy

3. **Security is Important**
   - JWT tokens for authentication
   - Password hashing with bcrypt
   - Role-based access control
   - Refresh tokens for persistent sessions

4. **Smart Features**
   - Smart scheduling algorithm recommends best slots
   - Real-time availability updates
   - Progress tracking with visual charts
   - Quiz system with immediate feedback

5. **Modern Technologies**
   - React for the frontend (component-based, fast)
   - TypeScript for type safety (fewer errors)
   - TailwindCSS for styling (faster development)
   - Express.js for the backend (proven, reliable)

### How to Explain the Flow

"When a student books a lesson:
1. The frontend (React) shows them available slots
2. When they click 'Book', it calls the backend API
3. The backend (Express) checks the database (SQLite via Prisma)
4. The backend creates a booking record
5. The backend sends a notification to the instructor
6. The frontend updates to show the booking is confirmed"

### What Makes This Project Special

- **Full-stack development** - You built both the frontend and backend
- **Real-world features** - Authentication, payments, messaging, file uploads
- **Smart algorithms** - The scheduling system that recommends optimal slots
- **Security best practices** - JWT tokens, password hashing, role-based access
- **Modern development** - Using current industry-standard technologies
- **Scalable architecture** - Easy to add new features or scale up

---

## Conclusion

DriveMate is a complete, production-ready web application that demonstrates:

- **Full-stack development skills** - You can build both the visible part (frontend) and the invisible part (backend)
- **Database design** - You understand how to structure data effectively
- **API development** - You know how to create and use RESTful APIs
- **Security implementation** - You understand authentication and authorization
- **Modern frameworks** - You're using current, industry-standard tools
- **Problem-solving** - You've built real solutions (like smart scheduling)

The system is ready to be deployed and used, and it can easily be extended with new features or scaled for larger use.

---

## Quick Reference for Your Presentation

**Backend Technologies:**
- Node.js, Express.js, Prisma ORM, SQLite, JWT, Passport.js, bcryptjs

**Frontend Technologies:**
- React, TypeScript, TailwindCSS, React Router, Axios, Lucide React, Recharts

**Database:**
- SQLite with 12 models (User, StudentProgress, AvailabilitySlot, Booking, Payment, Message, Feedback, QuizAttempt, Notification, RefreshToken, Vehicle, Document)

**Key Files:**
- `backend/server.js` - Main backend (1500+ lines)
- `backend/prisma/schema.prisma` - Database schema
- `frontend/src/App.tsx` - Main frontend with routing
- `frontend/src/context/AuthContext.tsx` - Authentication state
- `frontend/src/lib/api.ts` - API client

**Access URL:**
- http://localhost:5000

**Test Accounts:**
- Admin: admin@drivemate.com / admin123
- Instructor: instructor@drivemate.com / instructor123
- Student: student@drivemate.com / student123

## Project Architecture

### Directory Structure

```
Drivemate2/
├── backend/                    # Backend API server
│   ├── config/
│   │   └── passport.js        # Passport authentication strategies
│   ├── prisma/
│   │   ├── schema.prisma     # Database schema definition
│   │   ├── seed.js           # Database seeding script
│   │   └── dev.db            # SQLite database file
│   ├── server.js             # Main Express server (1500+ lines)
│   ├── .env                  # Environment variables
│   └── package.json          # Backend dependencies
├── frontend/                   # React frontend application
│   ├── src/
│   │   ├── context/          # React Context providers
│   │   │   ├── AuthContext.tsx    # Authentication state management
│   │   │   └── ThemeContext.tsx   # Theme state management
│   │   ├── lib/              # Utility libraries
│   │   │   └── api.ts        # Axios API client with endpoints
│   │   ├── pages/            # Page components
│   │   │   ├── LandingPage.tsx       # Public landing page
│   │   │   ├── LoginPage.tsx         # Authentication page
│   │   │   ├── StudentDashboard.tsx  # Student main interface
│   │   │   ├── InstructorDashboard.tsx # Instructor main interface
│   │   │   ├── AdminDashboard.tsx    # Admin management interface
│   │   │   ├── BookingPage.tsx       # Lesson booking system
│   │   │   ├── PaymentsPage.tsx      # Payment management
│   │   │   ├── ProgressPage.tsx      # Student progress tracking
│   │   │   ├── QuizPage.tsx          # License code quiz
│   │   │   ├── MessagesPage.tsx      # Messaging system
│   │   │   ├── NotificationsPage.tsx  # Notification center
│   │   │   ├── VehiclesPage.tsx      # Vehicle fleet management
│   │   │   ├── DocumentsPage.tsx     # Document upload/verification
│   │   │   ├── AttendancePage.tsx     # Attendance tracking
│   │   │   └── ProfilePage.tsx       # User profile management
│   │   ├── types/            # TypeScript type definitions
│   │   │   └── index.ts      # Shared interfaces
│   │   ├── App.tsx           # Main application component with routing
│   │   ├── index.css         # Global styles with Tailwind
│   │   └── index.js          # React entry point
│   ├── public/               # Static assets
│   ├── tailwind.config.js    # Tailwind configuration
│   ├── tsconfig.json         # TypeScript configuration
│   └── package.json          # Frontend dependencies
├── package.json              # Root package with orchestration scripts
└── README_PRESENTATION.md    # This file
```

## System Architecture

### Backend Architecture (server.js)

The backend follows a **monolithic REST API architecture** with the following key components:

**Location:** `backend/server.js` (1533 lines)

#### 1. Middleware Setup
- **CORS** - Enables cross-origin requests from frontend
- **Express JSON** - Parses incoming JSON request bodies
- **Passport Initialization** - Sets up authentication strategies

#### 2. Authentication Layer
- **JWT Token Verification** - Custom middleware (`authenticateToken`) validates Bearer tokens
- **Role-Based Authorization** - Helper function (`authorize`) restricts access by user roles
- **Token Storage** - Refresh tokens stored in database for session persistence

#### 3. API Endpoints

**Authentication Endpoints:**
- `POST /api/auth/register` - User registration with password hashing
- `POST /api/auth/login` - Local strategy authentication with JWT generation
- `GET /api/auth/me` - Current user profile retrieval
- `POST /api/auth/refresh` - JWT token refresh using refresh token

**User Management (Admin):**
- `GET /api/admin/users` - Paginated user listing
- `PATCH /api/admin/users/:id` - Role assignment and profile updates
- `DELETE /api/admin/users/:id` - User deletion

**Availability Slots (Instructor):**
- `POST /api/slots` - Create available time slots
- `GET /api/slots/available` - Query available slots with filters
- `GET /api/slots/my-schedule` - Instructor's schedule retrieval
- `DELETE /api/slots/:id` - Slot cancellation

**Booking System:**
- `POST /api/bookings/:slotId` - Book a lesson slot
- `GET /api/bookings/my-lessons` - Student's lesson history
- `POST /api/bookings/:id/cancel` - Booking cancellation

**Progress Tracking:**
- `GET /api/progress/:studentId` - Student progress details
- `PATCH /api/progress/:studentId` - Update progress metrics
- `GET /api/lessons/history` - Lesson history with attendance
- `GET /api/lessons/credits` - Credit balance calculation

**Payment Management:**
- `GET /api/payments` - Payment history
- `POST /api/payments` - Payment processing
- `PATCH /api/payments/:id` - Payment status updates

**Messaging System:**
- `GET /api/messages/threads` - Conversation listing
- `GET /api/messages/:peerId` - Message history with specific user
- `POST /api/messages` - Send message
- `PATCH /api/messages/:id/read` - Mark as read

**Quiz System:**
- `GET /api/quiz/questions` - Retrieve quiz questions by license code
- `POST /api/quiz/attempt` - Submit quiz attempt

**Notification System:**
- `GET /api/notifications` - User notifications
- `PATCH /api/notifications/:id/read` - Mark notification as read

**Document Management:**
- `POST /api/documents/upload` - Document upload with validation
- `GET /api/documents` - User's documents
- `PATCH /api/documents/:id/review` - Admin document review

**Vehicle Management:**
- `GET /api/vehicles` - Vehicle fleet listing
- `POST /api/vehicles` - Add new vehicle
- `PATCH /api/vehicles/:id` - Update vehicle details
- `DELETE /api/vehicles/:id` - Remove vehicle

**Attendance Tracking:**
- `POST /api/attendance/check-in` - Record lesson attendance
- `GET /api/attendance` - Attendance records

#### 4. Smart Scheduling Algorithm
**Location:** `server.js` lines 74-120

A sophisticated scoring system that recommends optimal lesson slots based on:
- **Temporal proximity** - Prefer nearer dates
- **Time preferences** - Morning (8-9 AM) and afternoon (2-3 PM) slots scored higher
- **Student progress** - Beginners (<50% progress) receive priority
- **Instructor availability** - Real-time slot availability checks

### Database Schema (Prisma)

**Location:** `backend/prisma/schema.prisma`

The database uses **SQLite** with 12 interconnected models:

#### Core Models

**User Model** - Central user entity with role-based access
```prisma
- id, email, name, passwordHash, role (STUDENT/INSTRUCTOR/ADMIN)
- Relations: bookings, messages, payments, notifications, documents, attendance
```

**StudentProgress Model** - Tracks student learning journey
```prisma
- studentId, track (CODE_8/CODE_10), progressPct, rating, completedLessons
- Relations: feedback, user
```

**AvailabilitySlot Model** - Instructor availability management
```prisma
- instructorId, date, timeWindow, vehicle, isBooked
- Unique constraint: [instructorId, date, timeWindow, vehicleId]
- Relations: booking, vehicle, instructor
```

**Booking Model** - Lesson booking records
```prisma
- slotId, studentId, status (CONFIRMED/CANCELLED/COMPLETED)
- Relations: slot, student, attendance
```

**Payment Model** - Financial transactions
```prisma
- studentId, amount, status, packageType, lessonsIncluded
- Package types: single, starter, pro, full
```

**Message Model** - Internal messaging
```prisma
- senderId, receiverId, content, isRead
- Relations: sender, receiver
```

**Feedback Model** - Lesson evaluations
```prisma
- studentId, authorId, ratingScore (1-5), evaluationNotes
- Relations: student, author
```

**QuizAttempt Model** - License code assessments
```prisma
- userId, licenseCode (8/10), score, percentScore, passed, answers (JSON)
```

**Notification Model** - User notifications
```prisma
- userId, type, title, message, isRead, relatedId
- Types: booking_confirmed, lesson_reminder, payment_received, etc.
```

**RefreshToken Model** - JWT refresh token storage
```prisma
- token (unique), userId, expiresAt
```

**Vehicle Model** - Fleet management
```prisma
- make, model, year, licensePlate (unique), vehicleType, code
- status: AVAILABLE, IN_USE, MAINTENANCE, OUT_OF_SERVICE
- Relations: availability slots
```

**Document Model** - Document verification
```prisma
- userId, documentType, fileName, fileUrl, status
- Types: ID, LEARNERS_LICENSE, MEDICAL_CERTIFICATE, PROOF_OF_ADDRESS
- Status: PENDING, APPROVED, REJECTED
```

**Attendance Model** - Lesson attendance records
```prisma
- bookingId (unique), studentId, instructorId, status
- Status: PRESENT, ABSENT, CANCELLED, NO_SHOW
- checkInTime, checkOutTime, notes
```

### Frontend Architecture

#### Authentication Context
**Location:** `frontend/src/context/AuthContext.tsx`

Implements React Context API for global authentication state:
- Manages user session and JWT token
- Provides login, register, logout functions
- Auto-loads user profile on app start
- Persists token in localStorage

#### API Client
**Location:** `frontend/src/lib/api.ts`

Centralized Axios instance with:
- Base URL configuration
- Request interceptor for automatic token injection
- Organized API modules by feature:
  - `authAPI` - Authentication endpoints
  - `usersAPI` - User management
  - `slotsAPI` - Availability slots
  - `bookingsAPI` - Booking operations
  - `progressAPI` - Progress tracking
  - `paymentsAPI` - Payment operations
  - `messagesAPI` - Messaging
  - `notificationsAPI` - Notifications
  - `quizAPI` - Quiz system
  - `documentsAPI` - Document management
  - `vehiclesAPI` - Vehicle management
  - `attendanceAPI` - Attendance tracking

#### Type Definitions
**Location:** `frontend/src/types/index.ts`

TypeScript interfaces ensuring type safety across the application:
- User, StudentProgress, AvailabilitySlot, Booking
- Payment, Message, Feedback, Notification
- QuizAttempt, QuizQuestion, MessageThread
- SmartSchedulingSuggestion, LessonCredits
- AuthContextType

#### Routing & Navigation
**Location:** `frontend/src/App.tsx`

React Router implementation with:
- Protected routes for authenticated users
- Role-based route guards (STUDENT, INSTRUCTOR, ADMIN)
- Dynamic navigation based on user role
- Loading states during authentication

#### Page Components

**Landing Page** (`LandingPage.tsx` - 34,786 bytes)
- Public-facing marketing page
- Feature highlights and pricing
- Call-to-action for registration

**Login Page** (`LoginPage.tsx`)
- Authentication form with validation
- Error handling and feedback

**Student Dashboard** (`StudentDashboard.tsx`)
- Overview of student's learning journey
- Quick access to bookings, progress, payments
- Upcoming lessons display

**Instructor Dashboard** (`InstructorDashboard.tsx`)
- Schedule management interface
- Student assignment overview
- Quick access to attendance and feedback

**Admin Dashboard** (`AdminDashboard.tsx`)
- System-wide statistics
- User management overview
- Fleet and financial summaries

**Booking Page** (`BookingPage.tsx`)
- Calendar-based slot selection
- Smart scheduling recommendations
- Real-time availability display

**Payments Page** (`PaymentsPage.tsx`)
- Payment history with filters
- Package purchase interface
- Credit balance display

**Progress Page** (`ProgressPage.tsx`)
- Visual progress tracking with charts
- Lesson history and feedback
- Rating and performance metrics

**Quiz Page** (`QuizPage.tsx`)
- Interactive quiz interface
- Code 8 and Code 10 question sets
- Immediate scoring and feedback

**Messages Page** (`MessagesPage.tsx`)
- Threaded messaging interface
- Real-time unread indicators
- Conversation history

**Notifications Page** (`NotificationsPage.tsx`)
- Centralized notification center
- Read/unread status management
- Related resource navigation

**Vehicles Page** (`VehiclesPage.tsx`)
- Fleet inventory management
- Vehicle status tracking
- Maintenance scheduling

**Documents Page** (`DocumentsPage.tsx`)
- Document upload with validation
- Status tracking (PENDING/APPROVED/REJECTED)
- Admin review interface

**Attendance Page** (`AttendancePage.tsx`)
- Check-in/check-out functionality
- Attendance history and statistics
- Instructor notes recording

**Profile Page** (`ProfilePage.tsx`)
- User profile management
- Personal information updates
- Password change functionality

## Authentication Flow

### Registration Process
1. User submits registration form via `LoginPage.tsx`
2. Frontend calls `authAPI.register()` → `POST /api/auth/register`
3. Backend hashes password with bcrypt
4. Creates User record in database
5. Generates JWT access token and refresh token
6. Returns user data and tokens to frontend
7. Frontend stores token in localStorage and updates AuthContext

### Login Process
1. User submits credentials via `LoginPage.tsx`
2. Frontend calls `authAPI.login()` → `POST /api/auth/login`
3. Passport Local Strategy validates credentials
4. Compares hashed password using bcrypt
5. Generates JWT access token (short-lived) and refresh token (long-lived)
6. Returns tokens and user data
7. Frontend stores tokens and updates AuthContext

### Token Refresh Flow
1. When access token expires, frontend calls `/api/auth/refresh`
2. Backend validates refresh token from database
3. Issues new access token if refresh token is valid
4. Updates refresh token expiration
5. Returns new access token

### Protected Route Access
1. User navigates to protected route
2. `ProtectedRoute` component checks AuthContext
3. If no token, redirects to login
4. If token exists, calls `/api/auth/me` to validate
5. Checks role permissions if required
6. Renders component or redirects based on authorization

## Key Implementation Details

### Smart Scheduling Algorithm
**Location:** `backend/server.js` lines 74-120

The algorithm implements a weighted scoring system:
- **Base score**: 100 points
- **Date proximity penalty**: -8 points per day away
- **Time preference bonus**: +8 for morning slots, +5 for afternoon
- **Beginner priority**: +10 for students with <50% progress
- **Maximum penalty**: 60 points to prevent negative scores

This ensures students get optimal slots while considering their learning stage.

### Role-Based Access Control
**Location:** `backend/server.js` lines 58-65

The `authorize()` middleware function:
- Accepts array of allowed roles
- Checks `req.user.role` from JWT payload
- Returns 403 Forbidden if unauthorized
- Allows access if role matches

Applied to sensitive endpoints like:
- Admin user management
- Instructor schedule creation
- Admin document review

### Database Relationships
**Location:** `backend/prisma/schema.prisma`

Key relationships implemented:
- User → StudentProgress (1:1) - Each student has one progress record
- User → AvailabilitySlot (1:N) - Instructors can have multiple slots
- AvailabilitySlot → Booking (1:1) - Each slot can have one booking
- Booking → Attendance (1:1) - Each booking has one attendance record
- User → Message (N:N) - Users can send/receive multiple messages
- User → Payment (1:N) - Students can have multiple payments
- Vehicle → AvailabilitySlot (1:N) - Vehicles can be assigned to multiple slots

Cascade deletes ensure data integrity when users or related records are removed.

### State Management
**Location:** `frontend/src/context/`

- **AuthContext**: Global authentication state (user, token, loading)
- **ThemeContext**: UI theme preferences (light/dark mode)

Context providers wrap the application in `App.tsx`, enabling any component to access state via custom hooks (`useAuth()`, `useTheme()`).

### API Error Handling
**Location:** `frontend/src/lib/api.ts`

Axios interceptors handle:
- Automatic token injection in request headers
- 401 responses trigger logout and redirect to login
- Network errors display user-friendly messages
- Validation errors show field-specific feedback

### Responsive Design
**Location:** `frontend/src/index.css` and Tailwind configuration

- Mobile-first approach using Tailwind breakpoints
- Responsive navigation with collapsible sidebar
- Adaptive layouts for dashboard widgets
- Touch-friendly interfaces for mobile users

## Data Flow Examples

### Booking a Lesson
1. Student navigates to `BookingPage.tsx`
2. Frontend calls `slotsAPI.getAvailable()` → `GET /api/slots/available`
3. Backend queries unbooked slots from database
4. Frontend displays available slots with smart recommendations
5. Student selects slot and confirms
6. Frontend calls `bookingsAPI.book(slotId)` → `POST /api/bookings/:slotId`
7. Backend creates Booking record, updates AvailabilitySlot.isBooked
8. Backend creates Notification for instructor
9. Frontend updates UI and redirects to dashboard

### Submitting a Quiz
1. Student navigates to `QuizPage.tsx`
2. Frontend calls quiz API to get questions for selected license code
3. Student answers questions interactively
4. Frontend calculates score locally
5. Frontend calls quiz API to submit attempt
6. Backend creates QuizAttempt record with answers (JSON)
7. Backend determines pass/fail based on percentage
8. Frontend displays results with feedback

### Document Verification
1. Student uploads document via `DocumentsPage.tsx`
2. Frontend validates file type and size
3. Frontend calls `documentsAPI.upload()` → `POST /api/documents/upload`
4. Backend saves file, creates Document record with PENDING status
5. Admin navigates to DocumentsPage
6. Admin reviews document and approves/rejects
7. Backend updates Document status and reviewedBy/reviewedAt
8. Student receives notification of approval/rejection

## Security Measures

### Password Security
- Passwords hashed using bcrypt with salt rounds
- Never store plain-text passwords
- Password strength validation on registration

### Token Security
- JWT access tokens with short expiration (1 hour)
- Refresh tokens stored in database with expiration
- Token invalidation on logout
- Automatic token refresh mechanism

### API Security
- CORS configured to allow only frontend origin
- Rate limiting on sensitive endpoints
- Input validation and sanitization
- SQL injection prevention via Prisma ORM

### Role-Based Security
- Three-tier role system (STUDENT, INSTRUCTOR, ADMIN)
- Middleware enforces role permissions
- Frontend route guards prevent unauthorized access
- Backend validation on all protected endpoints

## Deployment Instructions

### Prerequisites
- Node.js (v18 or higher)
- npm or yarn package manager

### Installation Steps

1. **Clone the repository**
```bash
git clone <repository-url>
cd Drivemate2/Drivemate2
```

2. **Install dependencies**
```bash
npm run setup
```
This installs root, backend, and frontend dependencies, and generates Prisma client.

3. **Configure environment variables**
Create `backend/.env` file:
```
PORT=5000
JWT_SECRET=your-secret-key-here
DATABASE_URL=file:./prisma/dev.db
```

4. **Setup database**
```bash
npm run db:setup
```
This pushes schema to SQLite and seeds initial data.

5. **Run development servers**
```bash
npm run dev
```
This starts both backend (port 5000) and frontend (port 3000) concurrently.

6. **Build for production**
```bash
npm run build
npm start
```
This builds React app and serves it via Express.

### Database Management

**View database in Prisma Studio:**
```bash
npm run db:studio
```

**Reset database:**
```bash
npm run db:push
npm run db:seed
```

## Testing Accounts

The system includes seeded accounts for testing:

**Admin Account:**
- Email: admin@drivemate.com
- Password: admin123
- Role: Full system access

**Instructor Account:**
- Email: instructor@drivemate.com
- Password: instructor123
- Role: Schedule management, attendance, feedback

**Student Account:**
- Email: student@drivemate.com
- Password: student123
- Role: Book lessons, track progress, take quizzes

## Key Features Summary

### For Students
- Browse and book available lesson slots
- Track learning progress with visual charts
- Take license code quizzes (Code 8, Code 10)
- Manage payments and view credit balance
- Communicate with instructors via messaging
- Upload required documents for verification
- View attendance history

### For Instructors
- Create and manage availability slots
- View assigned students and schedules
- Record lesson attendance
- Provide feedback and ratings
- Communicate with students
- View vehicle assignments

### For Administrators
- Manage user accounts and roles
- Oversee system-wide operations
- Review and approve documents
- Manage vehicle fleet
- Monitor payments and revenue
- Access comprehensive analytics

## Technical Highlights

### Scalability Considerations
- SQLite can be replaced with PostgreSQL for production
- Modular API structure allows easy feature additions
- State management via Context API (can migrate to Redux for complex state)
- Component-based architecture enables code reuse

### Performance Optimizations
- Database indexing on frequently queried fields
- Lazy loading of dashboard components
- API response pagination for large datasets
- Optimistic UI updates for better user experience

### Code Quality
- TypeScript for type safety
- ESLint and Prettier for code formatting
- Modular component structure
- Separation of concerns (API, UI, State)
- Comprehensive error handling

## Future Enhancements

Potential improvements for future iterations:
- Real-time notifications via WebSocket
- Integration with payment gateways (PayPal, Stripe)
- Mobile app development (React Native)
- Advanced analytics and reporting
- Email notifications for important events
- Calendar integration (Google Calendar, Outlook)
- Multi-language support
- Video lessons and tutorials

## Conclusion

DriveMate demonstrates a full-stack web application with:
- **Separation of concerns** between frontend and backend
- **RESTful API design** for scalability
- **Type-safe development** with TypeScript
- **Modern UI/UX** with React and TailwindCSS
- **Secure authentication** with JWT and refresh tokens
- **Role-based access control** for different user types
- **Comprehensive data modeling** with Prisma ORM
- **Smart algorithms** for optimal user experience

The system is production-ready for local deployment and can be easily adapted for cloud hosting with minor configuration changes.
