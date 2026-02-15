Face Attendance System
📌 Overview
A full‑stack face recognition attendance system built with Node.js, Express, MongoDB, React, and face-api.js.
It allows administrators to register users, enroll faces, and track attendance in real time using a webcam. The system supports weekly attendance reports and live socket‑based alerts.

🚀 Features
- User Management: Admins can register and manage student accounts.
- Face Enrollment: Capture multiple samples of a user’s face for reliable recognition.
- Attendance Tracking: Real‑time face detection and recognition with automatic check‑in.
- Weekly Reports: Cron job generates attendance summaries and emails them to class teachers.
- Live Alerts: Socket.IO broadcasts attendance events to subscribed frontends.
- Secure Authentication: Role‑based access (admin vs. student).

🛠️ Tech Stack
- Backend: Node.js, Express, MongoDB, Mongoose
- Frontend: React, face-api.js
- Realtime: Socket.IO
- Scheduling: Node Cron
- Environment Config: dotenv

⚙️ Setup Instructions
1. MongoDB
- Ensure MongoDB is installed and running at mongodb://localhost:27017.
- Create database face_attendance or update URI in .env.
2. Backend
cd backend
cp .env.example .env   # Fill in environment variables
npm install
npm run dev            # or npm start


Backend runs at: http://localhost:5000
3. Frontend
cd frontend
npm install


- Download face-api.js models and place them in frontend/public/models/:
- tiny_face_detector_model-weights_manifest.json + binaries
- face_landmark_68_model-weights_manifest.json + binaries
- face_recognition_model-weights_manifest.json + binaries
- Start frontend:
npm start


Frontend runs at: http://localhost:3000

👤 Admin Setup
- Register admin via:
POST /api/auth/register
{
  "name": "Admin",
  "email": "admin@example.com",
  "password": "pass123",
  "role": "admin"
}


- Login at /login with these credentials.

📷 Usage Flow
- Enroll Faces: Admin creates student users → /enroll → capture 3–6 samples → save descriptors.
- Take Attendance: Visit /live → system detects faces → matches descriptors → logs attendance.
- Reports: Weekly cron job emails attendance summaries.
- Live Updates: Socket.IO pushes attendance events to connected clients.

📧 Weekly Reports
- Cron job runs every Friday at 17:00 (server time).
- Generates last 7 days’ attendance per class.
- Emails report to class teacher.
- Configurable in jobs/weeklyReportJob.js.

🔔 Real‑Time Alerts
- Attendance events emitted via Socket.IO.
- Frontend subscribes to show live updates instantly.


