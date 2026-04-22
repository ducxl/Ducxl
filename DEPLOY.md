# DUCXL — Go Live Free in 15 Minutes

No server. No backend cost. No credit card. 100% free forever.

---

## How it works (cost breakdown)

| Service        | What it does              | Cost     |
|----------------|---------------------------|----------|
| GitHub Pages   | Hosts your website        | **FREE** |
| Firebase Auth  | Login / Signup            | **FREE** |
| Firebase Firestore | Projects & payments DB | **FREE** |
| Firebase Hosting (alt) | Deploy from CLI   | **FREE** |
| Total          |                           | **₹0/month** |

---

## Step 1 — Create Firebase Project (5 min)

1. Go to https://console.firebase.google.com
2. Click **"Add project"** → name it `ducxl` → Create
3. In the left sidebar → **Authentication** → Get Started → Enable **Email/Password**
4. In the left sidebar → **Firestore Database** → Create database → **Start in test mode** → Choose a region → Done
5. Click the gear icon → **Project Settings** → scroll to **"Your apps"**
6. Click **`</>`** (Web app) → Register app as `ducxl-web` → Copy the config object that appears

Your config looks like this:
```js
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "ducxl-xxxxx.firebaseapp.com",
  projectId: "ducxl-xxxxx",
  storageBucket: "ducxl-xxxxx.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

---

## Step 2 — Configure index.html (2 min)

Open `index.html` and find this section near the bottom (inside the `<script type="module">`):

```js
const FIREBASE_CONFIG = {
  apiKey:            "YOUR_API_KEY",       // ← paste your values here
  authDomain:        "YOUR_PROJECT.firebaseapp.com",
  projectId:         "YOUR_PROJECT_ID",
  storageBucket:     "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId:             "YOUR_APP_ID"
};

const ADMIN = "skdrago777@gmail.com";     // ← your admin email
```

Replace each `"YOUR_..."` value with what Firebase gave you. Save the file.

---

## Step 3 — Deploy to GitHub Pages (5 min)

### Option A: GitHub website (no command line needed)

1. Go to https://github.com → Sign in (or create free account)
2. Click **"New repository"** → name it `ducxl` → Public → Create
3. Click **"uploading an existing file"** link
4. Drag your `index.html` file into the upload area → Commit changes
5. Go to **Settings** → **Pages** (left sidebar)
6. Under "Source" → select **Deploy from branch** → branch: `main` → folder: `/ (root)` → Save
7. Wait ~60 seconds → your site is live at:

```
https://YOUR-GITHUB-USERNAME.github.io/ducxl
```

### Option B: GitHub CLI (if you have git installed)

```bash
cd ducxl-free
git init
git add index.html
git commit -m "launch DUCXL"
git branch -M main
git remote add origin https://github.com/YOUR-USERNAME/ducxl.git
git push -u origin main
# Then enable Pages in GitHub repo Settings → Pages
```

---

## Step 4 — Set Firestore Security Rules (2 min)

In Firebase Console → Firestore → **Rules** tab → paste:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /users/{userId} {
      allow read, write: if request.auth.uid == userId;
    }
    match /projects/{docId} {
      allow create: if request.auth != null;
      allow read, update: if request.auth.uid == resource.data.userId
        || request.auth.token.email == "skdrago777@gmail.com";
    }
    match /payments/{docId} {
      allow create: if request.auth != null;
      allow read, update: if request.auth.token.email == "skdrago777@gmail.com"
        || request.auth.uid == resource.data.userId;
    }
  }
}
```

Click **Publish**.

---

## Step 5 — Add Authorised Domain (1 min)

In Firebase Console → Authentication → **Settings** → **Authorised domains** → Add domain:

```
YOUR-GITHUB-USERNAME.github.io
```

This allows your GitHub Pages URL to use Firebase Auth.

---

## Your site is now live! 🎉

URL: `https://YOUR-GITHUB-USERNAME.github.io/ducxl`

**What works out of the box:**
- ✅ Full landing page with all sections
- ✅ Client signup & login
- ✅ Project submission form
- ✅ Real-time project status tracking
- ✅ UPI payment confirmation flow
- ✅ Admin dashboard (login with your admin email)
- ✅ All data stored in Firestore

---

## Want a custom domain later? (Also free!)

If you later buy `ducxl.com` (~₹850/year on Namecheap):

1. In your GitHub repo → Settings → Pages → Custom domain → enter `ducxl.com`
2. In your domain registrar DNS settings, add:
   - A records pointing to GitHub's IPs (GitHub gives you these)
3. Firebase Console → Authentication → Authorized domains → add `ducxl.com`

Done. Your site moves from `github.io/ducxl` to `ducxl.com`.

---

## Admin Usage

1. First, create an account with your admin email (`skdrago777@gmail.com`)
2. Log in — you'll automatically see the Admin Panel
3. From there you can:
   - See all submitted projects
   - Mark them Approved or Complete
   - See all payment submissions
   - Approve payments (activates user subscription in Firestore)
