# Chinese Word Reading – setup

## Excel format
Row 1 holds the headings (any column order; Chinese headings such as 年级 / 册 / 课 / 词语 also work). One word per row after that:

| Level | Book | Chapter | Chapter title | Word | Pinyin (optional) | Meaning (optional) |
|-------|------|---------|---------------|------|-------------------|--------------------|
| P2    | A    | 9       | 今天是晴天     | 乌云 | wū yún            | dark clouds        |

- **Level**: P1–P6 (`2`, `Primary 2` also work). **Book**: A or B. **Chapter**: the number (`9`, `09` or `2A_09`).
- The app shows that chapter as **2A_09 《今天是晴天》**, the same as the label on the flash card.
- One file can hold several books (for example P2 Book A and Book B). Uploading a book again **replaces** that book: words no longer in the file are removed with their progress; words still in it keep their progress.
- Rows with a missing or unclear Level, Book or Chapter are skipped, and the app says which rows.
- `.xlsx`, `.xls` and `.csv` all work. The app has a "Download template" button.

## Using it
- **Upload File** (left) opens the upload page: choose the file, check the preview, then Save.
- **Level P1–P6** (left): pick a level, then Book A, Book B or Both books, then tick the chapters. The word counts and "How many words today?" follow what is ticked.
- Lists uploaded with the old one-column format (before version 8) are cleared when the child logs in; upload them again in the new format.

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
