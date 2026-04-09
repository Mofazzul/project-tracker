# ProjectTrack — Setup Guide

## Step 1 — Create Firebase Project

1. Go to [console.firebase.google.com](https://console.firebase.google.com)
2. Click **Add project** → give it a name → Continue
3. Disable Google Analytics (not needed) → **Create project**

---

## Step 2 — Enable Email/Password Auth

1. In the Firebase Console sidebar → **Authentication** → **Get started**
2. Click **Email/Password** → Enable → **Save**
3. Go to the **Users** tab → **Add user**
4. Enter **your email** and **a strong password** → **Add user**

> You can add more users here later if needed.

---

## Step 3 — Create Firestore Database

1. Sidebar → **Firestore Database** → **Create database**
2. Choose **Start in production mode** → Next → Select your region → **Done**
3. Go to the **Rules** tab and replace the content with:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId}/{document=**} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }
  }
}
```

4. Click **Publish**

---

## Step 4 — Get Your Firebase Config

1. Sidebar → ⚙️ **Project Settings** → scroll down to **Your apps**
2. Click the **`</>`** (Web) icon → Register app (name it anything)
3. Copy the `firebaseConfig` object shown

---

## Step 5 — Paste Config into index.html

Open `index.html` and find this section near the top of the `<script>`:

```js
const firebaseConfig = {
  // PASTE YOUR FIREBASE CONFIG HERE
};
```

Replace the comment with your actual config:

```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "your-project.firebaseapp.com",
  projectId: "your-project-id",
  storageBucket: "your-project.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123:web:abc123"
};
```

---

## Step 6 — Deploy to GitHub Pages

```bash
# In your project folder:
git init
git add index.html README.md
git commit -m "Initial commit"

# Push to GitHub (create a repo first at github.com/new)
git remote add origin https://github.com/YOUR_USERNAME/YOUR_REPO.git
git push -u origin main
```

Then in your GitHub repo:
- Go to **Settings** → **Pages**
- Source: **Deploy from a branch** → Branch: `main` / `/ (root)` → **Save**
- Your app is live at: `https://YOUR_USERNAME.github.io/YOUR_REPO/`

> ⚠️ GitHub Pages repos must be **public** on free accounts. Your Firebase config keys in the code are safe — Firebase security is enforced by Firestore Rules, not by keeping keys secret.

---

## How It Works

| Feature | Details |
|---|---|
| 🔐 Login | Firebase Auth — only accounts you create can log in |
| ☁️ Data | Stored in Firestore under your user ID — syncs across devices/browsers |
| 💾 Auto-save | Changes save automatically ~1 second after you make them |
| 📸 Screenshot | "Save as Image" on the Summary panel downloads a PNG |
| 🔄 Migration | If you had existing data in the old local version, it auto-migrates on first login |
