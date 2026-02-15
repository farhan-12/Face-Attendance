README / Setup & Run instructions
MongoDB
Ensure MongoDB is installed and running on mongodb://localhost:27017. Create database face_attendance or change URI in .env.
Backend
cd backend
cp .env.example .env and fill env variables (or create .env with values shown above).
npm install
npm run dev (or npm start)
Backend will run on http://localhost:5000.
Frontend
cd frontend
npm install
Download face-api.js models and put them in frontend/public/models/. The models you need:
tiny_face_detector_model-weights_manifest.json + binary shard files
face_landmark_68_model-weights_manifest.json + binaries
face_recognition_model-weights_manifest.json + binaries
You can find these in the face-api.js release storage (GitHub releases or a CDN). When in doubt, search “face-api.js models download” and follow their README. 
npm start
Frontend will run on http://localhost:3000.
Create initial users
Admin can be created by hitting /api/auth/register (POST) with JSON:
Json
Copy code
{ "name": "Admin", "email": "admin@example.com", "password": "pass123", "role": "admin" }
Use Postman or curl for quick setup. Or create a small seed script.
Enroll faces
Use the admin to create student users in DB.
Login as admin and open /enroll. Enter the target user's DB id (from GET /api/users), capture a few images (3–6 samples recommended from different angles/lighting), and click Save — this posts descriptors to backend.
Take attendance
Use /live. The page fetches stored descriptors, uses the camera to detect a face, compares the descriptor to stored ones (Euclidean distance) and, if recognized within threshold, calls POST /api/attendance/checkin to log attendance.
Weekly reports
The backend runs a cron job every Friday at 17:00 (server time) that compiles last 7 days attendance per class and emails the class teacher. Modify the cron schedule in jobs/weeklyReportJob.js as needed.
Socket-based alerts
The server uses Socket.IO to emit attendance events when new attendance is recorded; frontends can subscribe to show live updates.