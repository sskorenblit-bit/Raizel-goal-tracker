# Raizel's Goals 💫

A behavior tracking app for Raizel — earn money, build streaks, collect badges, and unlock rewards.

## Features

- 🌅 Daily behaviors (morning, after school, evening, weekends)
- 🏫 Smart school tracking (only weekdays, splits Math + Music on Tuesdays)
- 🕯️ Shabbat-specific behaviors on Saturdays
- ☀️ Sunday-specific behaviors
- 💛 Up to 3 kindness entries per day with journal
- 🧘‍♀️ Up to 3 coping skills per day with reflection prompts
- 🌜 Bedtime bonus (8:30 PM = $1.50, 9:00 PM = $0.75)
- 🚿 Extra shower bonus (Mon or Tue)
- 💰 Real money earnings + reward shop
- 🏆 Streak badges (3, 7, 14, 30, 60 days)
- 🔒 Parent dashboard behind PIN to edit prices/rewards
- ☁️ Cloud sync via Firebase (signs in across devices)

---

## Setup Instructions

### 1. Create a Firebase Project

1. Go to https://console.firebase.google.com
2. Click "Add project" → name it something like `raizel-goals`
3. Disable Google Analytics (not needed)
4. Once created, click the web icon `</>`  to add a web app
5. Register the app with a nickname like "Raizel Goals Web"
6. **Copy the firebaseConfig object** — you'll need it in step 4

### 2. Enable Authentication

1. In Firebase console → "Build" → "Authentication" → "Get started"
2. Click "Sign-in method" tab
3. Enable **Email/Password**
4. Click "Users" tab → "Add user" → enter Raizel's (or your) email + password

### 3. Enable Firestore Database

1. In Firebase console → "Build" → "Firestore Database" → "Create database"
2. Choose **Production mode**
3. Pick a location (closest to you)
4. Once created, go to "Rules" tab and paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
      match /{document=**} {
        allow read, write: if request.auth != null && request.auth.uid == userId;
      }
    }
  }
}
```

5. Click "Publish"

### 4. Configure the App

1. Open `public/firebase-config.js`
2. Replace the placeholder values with your firebaseConfig from step 1

### 5. Deploy to Netlify

#### Option A: Via GitHub (recommended)
1. Push this folder to a new GitHub repo
2. Go to https://app.netlify.com → "Add new site" → "Import an existing project"
3. Pick GitHub → select your repo
4. Build settings:
   - Build command: *(leave empty)*
   - Publish directory: `public`
5. Click "Deploy"

#### Option B: Drag & drop
1. Just drag the `public` folder onto https://app.netlify.com/drop

### 6. Add the Netlify URL to Firebase Authorized Domains

1. Once deployed, copy your Netlify URL (e.g., `raizel-goals.netlify.app`)
2. Firebase console → Authentication → Settings → "Authorized domains"
3. Add the Netlify domain

Done! 🎉

---

## How to Use

- **First time:** Sign in with the email/password you created in Firebase
- **Daily:** Open the app, check off behaviors, earn money
- **Parent dashboard:** Tap "👤 Parent" at bottom of page → enter PIN (default: `1234`)
- **Change PIN:** In parent dashboard, scroll to "🔑 Change Parent PIN"

## Data Sync

All data syncs to Firebase Firestore automatically. Open on any device with the same login and you'll see the same data.

## File Structure

```
raizel-app/
├── README.md
├── netlify.toml
└── public/
    ├── index.html        # Main app
    ├── login.html        # Sign-in page
    ├── firebase-config.js  # Your Firebase credentials (EDIT THIS)
    └── app.js            # App logic with Firebase sync
```
