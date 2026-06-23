# J vs M Challenge — Hosting & Setup

This single-file site (index.html) tracks daily results and can be hosted publicly on GitHub Pages. It includes an optional Firestore integration for shared, no-auth entries.

## Quick repo push (local)
1. Install Git and GitHub CLI if needed:

   - Git: https://git-scm.com/download/win
   - GitHub CLI: https://cli.github.com/

2. From PowerShell in the project folder:

```powershell
cd "C:\Users\jonbo\Documents\Websites"
git init -b main
git add .
git commit -m "Initial commit: J vs M Challenge (Firestore integration)"
gh repo create the-vineyard --public --source=. --remote=origin --push
# OR if you created the repo on github.com manually:
# git remote add origin https://github.com/<your-username>/the-vineyard.git
# git push -u origin main
```

## Enable GitHub Pages
1. Go to the repo on GitHub → Settings → Pages
2. Choose branch `main` and folder `/ (root)`, Save. The site will be available at `https://<your-username>.github.io/the-vineyard/`.

## Firebase Firestore (public, no-auth)
This project includes a small Firestore listener and uses the `entries` collection for shared data.

1. Create a Firebase project at https://console.firebase.google.com/
2. Create a Firestore database (Native mode).
3. In Project Settings → SDK config, copy the config values and replace the `firebaseConfig` object in `index.html`.

Example config (replace with your values):

```js
const firebaseConfig = {
  apiKey: 'YOUR_API_KEY',
  authDomain: 'YOUR_PROJECT.firebaseapp.com',
  projectId: 'YOUR_PROJECT_ID'
};
```

4. (Optional / required if you want truly public writes) Update Firestore rules for testing only:

```
service cloud.firestore {
  match /databases/{database}/documents {
    match /entries/{doc} { allow read, write: if true; }
  }
}
```

Warning: Allowing public writes makes your database writable by anyone. For production, consider adding validation rules or simple token-based checks.

## How the integration works
- The page will use Firestore when a valid `firebaseConfig.projectId` is present.
- A realtime listener keeps `entries` synced and updates charts automatically.
- If Firestore is not configured, the page falls back to `localStorage`.

## Testing
1. Open the site (locally or via GitHub Pages).
2. Add an entry from two different browsers or devices; if Firestore is configured and rules permit, both will sync in near real-time.

## Troubleshooting
- If nothing syncs, open browser DevTools Console for Firebase errors.
- Ensure `firebaseConfig` is correct and Firestore rules permit reads.
- If you prefer not to use Firestore, keep using the local file — entries are stored per-browser in `localStorage`.

## Next steps I can help with
- Push the repo for you (I attempted, but `git`/`gh` aren't available in this environment).
- Create a sample `firebaseConfig`-filled file if you provide your Firebase values.
- Lock down Firestore rules with basic validation.

---
Files: `index.html` — main site (edit firebaseConfig and Firestore rules as described).
