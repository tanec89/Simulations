# Chinese Word Reading – setup

## Excel format
One word per row. Columns (only the first is required):

| Word | Pinyin (optional) | Meaning (optional) |
|------|-------------------|--------------------|
| 苹果 | píng guǒ          | apple              |

A header row is detected and skipped. `.xlsx`, `.xls` and `.csv` all work. The app has a "Download template" button.

## Sync across devices (Firebase, free tier)
1. Go to <https://console.firebase.google.com> → **Add project** (no Analytics needed).
2. **Build → Authentication → Get started → Sign-in method → Email/Password → Enable → Save.**
   (Leave "Email link" off. The app turns each username into a made-up address, so nobody types or receives an email.)
3. **Build → Firestore Database → Create database** (production mode, any region).
4. Open the **Rules** tab and publish:
   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /flashcardProfiles/{uid} {
         allow read, write: if request.auth != null && request.auth.uid == uid;
       }
     }
   }
   ```
   Each login can only read and write its own record.
5. **Project settings (gear) → Your apps → Web (`</>`)** → register an app → copy the `firebaseConfig` object.
6. Paste it into `firebase-config.js` as `window.FIREBASE_CONFIG = {...};`, commit and push.
7. **Authentication → Settings → Authorized domains**: add your GitHub Pages domain (e.g. `yourname.github.io`).

### Accounts
- In the app: type a username and a password (at least 6 characters), then **Create new profile**. After that, **Log in** on any device.
- A device stays logged in until **Log out** is tapped.
- Forgotten password: there is no email, so it cannot be reset by the child. As administrator you can delete the user under **Authentication → Users** and create the profile again (progress for that profile is lost unless you copy its Firestore record to the new user's ID).
- Administrator view: **Authentication → Users** lists the accounts (shown as `u…@cwr.example.com`); **Firestore → flashcardProfiles** holds each profile's name, word list and progress. Passwords are never visible.

## iPad
- Open the GitHub Pages link in Safari and allow the microphone when asked.
- Settings → General → Keyboard → **Enable Dictation**, and add a Chinese keyboard so Mandarin dictation is available.
- Use the **Test microphone** button on the home screen first.
- Use Add to Home Screen for a full-screen app.
