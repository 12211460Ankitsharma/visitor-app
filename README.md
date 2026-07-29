# GateClear Visitor Management System
## Firebase + Firestore + Vercel Setup Guide

---

## 📁 Project Structure

```
visitor-app/
├── public/
│   └── index.html        ← Main app (frontend + Firebase logic)
├── firestore.rules       ← Database security rules
├── firestore.indexes.json
├── vercel.json           ← Vercel deployment config
└── README.md
```

---

## STEP 1 — Firebase Project Banao

1. https://console.firebase.google.com par jao
2. **"Add project"** click karo
3. Project name daalo (e.g. `gateclear-visitor`)
4. Google Analytics: OFF kar do (optional)
5. **"Create project"** click karo

---

## STEP 2 — Firestore Database Enable Karo

1. Left sidebar mein **"Firestore Database"** click karo
2. **"Create database"** click karo
3. Location: **asia-south1 (Mumbai)** select karo ✅
4. Security rules: **"Start in test mode"** select karo
5. **"Enable"** click karo

---

## STEP 3 — Firebase Config Copy Karo

1. Firebase Console → **Project Settings** (gear icon)
2. **"Your apps"** section mein **"</> Web"** click karo
3. App nickname daalo (e.g. `visitor-web`)
4. **"Register app"** click karo
5. `firebaseConfig` object copy karo — kuch aisa dikhega:

```javascript
const firebaseConfig = {
  apiKey: "AIzaSy...",
  authDomain: "gateclear-visitor.firebaseapp.com",
  projectId: "gateclear-visitor",
  storageBucket: "gateclear-visitor.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

---

## STEP 4 — index.html mein Config Daalo

`public/index.html` file kholo aur yeh section dhundo:

```javascript
// 🔥 APNA FIREBASE CONFIG YAHAN DAALO
const firebaseConfig = {
  apiKey: "YOUR_API_KEY",           ← Replace karo
  authDomain: "YOUR_PROJECT...",    ← Replace karo
  projectId: "YOUR_PROJECT_ID",     ← Replace karo
  storageBucket: "YOUR_PROJECT...", ← Replace karo
  messagingSenderId: "YOUR_...",    ← Replace karo
  appId: "YOUR_APP_ID"             ← Replace karo
};
```

Apna copied config paste karo.

---

## STEP 5 — Firestore Security Rules Update Karo

1. Firebase Console → Firestore → **"Rules"** tab
2. Saara purana text delete karo
3. `firestore.rules` file ka content paste karo
4. **"Publish"** click karo

---

## STEP 6 — Vercel pe Deploy Karo

### Option A: GitHub se (Recommended)

1. GitHub par new repository banao
2. Saari files push karo:
```bash
git init
git add .
git commit -m "Initial commit"
git remote add origin https://github.com/tumhara-username/visitor-app.git
git push -u origin main
```

3. https://vercel.com par jao → **"New Project"**
4. GitHub repo select karo
5. **"Deploy"** click karo
6. Done! URL milega jaise: `https://visitor-app.vercel.app`

### Option B: Vercel CLI se

```bash
npm install -g vercel
cd visitor-app
vercel
```

---

## STEP 7 — Firebase Domain Whitelist Karo

1. Firebase Console → **Authentication** → **Settings** → **Authorized domains**
2. Apna Vercel URL add karo (e.g. `visitor-app.vercel.app`)

---

## ✅ Kaise Kaam Karta Hai

```
Guard → Category select → Form fill → Submit
         ↓
   Firestore mein save hota hai
   (Collection: visitors, Document ID: auto)
         ↓
   WhatsApp message jaata hai visitor ko
   (Link: https://your-app.vercel.app?exit=DOCUMENT_ID)
         ↓
   Visitor link pe click karta hai → Out-Time dalta hai
         ↓
   Firestore mein outTime + status: "exited" update hota hai
```

---

## 🗄️ Firestore Data Structure

**Collection:** `visitors`

```json
{
  "appId": "I-4872",
  "category": "Interview",
  "categoryCode": "I",
  "name": "Rahul Sharma",
  "contact": "9876543210",
  "whom": "HR Department",
  "purpose": "Job Interview",
  "date": "2026-04-23",
  "inTime": "10:30",
  "outTime": "12:15",
  "photo": "base64...",
  "status": "inside | exited",
  "createdAt": "Timestamp",
  "exitedAt": "Timestamp"
}
```

---

## 🔒 Security Rules Summary

| Operation | Allowed |
|-----------|---------|
| Create new visitor | ✅ Yes |
| Read visitor data | ✅ Yes |
| Update outTime/status | ✅ Yes |
| Delete record | ❌ No |
| Update other fields | ❌ No |

---

## ❓ Common Issues

**Firebase config error?**
→ Check karo ki `YOUR_API_KEY` replace hua hai ya nahi

**WhatsApp link kaam nahi kar raha?**
→ Vercel URL deployed hona chahiye — localhost pe WhatsApp link kaam nahi karta

**Firestore permission denied?**
→ Security rules publish kiye? Test mode mein hai?

**Photo save nahi ho rahi?**
→ Base64 photo Firestore mein 1MB limit hai. Badi photos ke liye Firebase Storage use karo.
