# J vs M Challenge — How to Use

This repository is a single-file web application. The main page is [index.html](index.html) and it records daily results. It can run entirely in your browser (using localStorage) or optionally sync across devices via Firebase Firestore.

**Quick overview**
- Open the site in a browser to view, add, and edit entries.
- If you add a Firebase config, entries will sync in near real-time across devices.

## Quick Start (open in browser)
1. Double-click [index.html](index.html) or open it in your browser.
2. Use the UI to add an entry. Entries are saved to your browser's storage by default.

## Run a local static server (recommended for some browsers)
From the project folder run a simple server if you want proper CORS/Fetch behavior or test Firebase locally:

Windows (PowerShell):
```powershell
python -m http.server 8000
```
Then open `http://localhost:8000` in your browser.

## Using Firebase Firestore (optional)
1. Create a Firebase project at https://console.firebase.google.com/ and enable Firestore (Native mode).
2. In Project Settings → SDK config copy the config object.
3. Edit [index.html](index.html) and replace the `firebaseConfig` placeholder with your values.

Example:
```js
const firebaseConfig = {
  apiKey: 'YOUR_API_KEY',
  authDomain: 'YOUR_PROJECT.firebaseapp.com',
  projectId: 'YOUR_PROJECT_ID'
};
```

4. (Optional testing) If you want public writes for quick testing, set Firestore rules to allow reads/writes — only for testing:
```
service cloud.firestore {
  match /databases/{database}/documents {
    match /entries/{doc} { allow read, write: if true; }
  }
}
```
Warning: public writes are insecure. For production, add validation rules or authentication.

## What to expect
- If `firebaseConfig` is present and valid, the app will connect to Firestore and sync the `entries` collection in real time.
- If Firestore is not configured, the app stores data in `localStorage` (per-browser).

## Deploy (GitHub Pages)
1. Push the repo to GitHub.
2. In the repository Settings → Pages, set source to branch `main` and folder `/ (root)`.

Your site will be available at `https://<your-username>.github.io/<repo-name>/`.

## Troubleshooting
- No sync or Firebase errors: open DevTools Console and check Firebase messages.
- Verify `firebaseConfig` values and Firestore rules.
- If entries don't persist between sessions, ensure `localStorage` isn't disabled and you're not using a private browsing mode that clears storage.

## Development notes
- The whole app lives in [index.html](index.html). Edit it to change UI, labels, or Firebase behavior.
- Use a local HTTP server when testing features that rely on network requests.

## Want help?
- I can: push the repo to GitHub, help fill in `firebaseConfig` (if you share values), or suggest secure Firestore rules.

---
Files: [index.html](index.html) — main site.
