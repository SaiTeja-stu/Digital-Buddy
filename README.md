# Productivity Scheduler (React + Firebase)

Full-stack productivity app with:
- Firebase auth (email/password + Google)
- Private routes and session persistence
- Task scheduler with reminder timings
- Browser notifications (task + water reminders)
- Water intake tracking
- Rich text diary entries (Quill)
- Dashboard with daily summaries
- Responsive UI using Bootstrap + Tailwind
- Three.js animated background

## Folder Structure

```txt
src/
  components/
    AppLayout.jsx
    PrivateRoute.jsx
    ThreeBackground.jsx
  context/
    AuthContext.jsx
    ThemeContext.jsx
  firebase/
    config.js
  hooks/
    useNotifications.js
  pages/
    AuthPage.jsx
    DashboardPage.jsx
    DiaryPage.jsx
    SchedulerPage.jsx
    WaterPage.jsx
  router/
    AppRouter.jsx
  services/
    firestoreService.js
functions/
  index.js
  package.json
```

## Setup

1. Install dependencies:
   - `npm install`
2. Create `.env` from `.env.example`
3. Add your Firebase project values
4. Run frontend:
   - `npm run dev`

## Firebase Setup

1. Create a Firebase project
2. Enable Authentication providers:
   - Email/Password
   - Google
3. Create Firestore database (start in **production** mode, then add rules below)
4. **Publish Security Rules** (required — without the recursive `users/{uid}/**` rule, writes to diary/tasks/recycle bin are denied and the console stays empty):
   - This repo includes `firestore.rules` and `firebase.json` → run from project root:
     - `firebase login`
     - `firebase use focus-c038e` (or your project ID)
     - `firebase deploy --only firestore:rules`
   - Or in Firebase Console → Firestore → **Rules**, paste the contents of `firestore.rules` and **Publish**.
5. Firestore data model (collections appear after the first successful write):
   - Top-level: `users/{userId}` (profile), optional `visitorLogs`
   - Subcollections: `users/{userId}/tasks`, `diary`, `waterLogs`, `recycleBin`
   - In the console, open **`users`** → your user id document → subcollections (not only the root “Start collection” screen).

## Cloud Functions (Scheduled Reminder Example)

1. Install firebase CLI globally (`npm i -g firebase-tools`)
2. Login and initialize in project root:
   - `firebase login`
   - `firebase init functions`
3. Use provided `functions/index.js`
4. Deploy:
   - `cd functions`
   - `npm install`
   - `npm run deploy`

## Notifications

- Browser notifications are requested automatically after login.
- Task reminders and water reminders run client-side.
- `functions/index.js` includes a production path for scheduled checks.

## Optional Enhancements

- Firebase Cloud Messaging for push notifications across devices
- Offline cache and service worker for complete PWA support
