# Corcomm Blacklist

Tracks bad debt handed over to Sadler Inc and Du Randt & Louw for Corcomm Elektronies and Corcomm Sekuriteit.
The app runs on GitHub Pages. Logins, records and documents are kept in Firebase.

## What's in this folder

| File | What it is |
|---|---|
| `index.html` | The app |
| `firebase-config.js` | Your Firebase project's connection settings (you fill this in) |
| `firestore.rules` | Security rules for the records |
| `storage.rules` | Security rules for the uploaded documents |

---

## Part A – Firebase (about 15 minutes)

### 1. Create the project
1. Go to <https://console.firebase.google.com> and sign in with the Google account that should own Corcomm's data.
2. Click **Create a project** (or **Add project**). Name it `corcomm-blacklist`.
3. Google Analytics isn't needed; switch it off. Click **Create project**.

### 2. Register the web app
1. On the project home page, click the **web** icon `</>`.
2. Nickname: `Corcomm Blacklist`. Don't tick Firebase Hosting. Click **Register app**.
3. Firebase shows a block of code with `const firebaseConfig = { ... }`. Keep this page open; you need these values in step 6.

### 3. Switch on email and password logins
1. Left menu: **Build → Authentication → Get started**.
2. Under **Sign-in method**, click **Email/Password**, switch on the first toggle (not "Email link"), and click **Save**.

### 4. Create the database and publish its rules
1. Left menu: **Build → Firestore Database → Create database**.
2. Location: **africa-south1 (Johannesburg)**. This can't be changed later.
3. Choose **Start in production mode** and click **Create**.
4. Open the **Rules** tab, delete everything there, paste the entire contents of `firestore.rules`, and click **Publish**.

### 5. Set up document storage
Document storage needs Firebase's pay-as-you-go **Blaze** plan. Corcomm's volume stays within the free allowance (5 GB of files), so the expected cost is R0. A card is required.
1. Bottom-left, click **Upgrade** and choose **Blaze**. Link or create a billing account.
2. When asked, set a **budget alert** (for example R100) so you're emailed if anything ever costs money.
3. Left menu: **Build → Storage → Get started**. Choose **production mode**. Pick the same location as the database if it's offered.
4. Open the **Rules** tab, replace everything with the contents of `storage.rules`, and click **Publish**.
5. If Firebase asks to give Storage permission to read Firestore, click **Allow** / **Grant**. The rules check that the person is an active team user.

### 6. Paste your settings into `firebase-config.js`
Open `firebase-config.js` in Notepad and replace each value with the matching one from step 2:

```js
window.FIREBASE_CONFIG = {
  apiKey: "AIza…",
  authDomain: "corcomm-blacklist.firebaseapp.com",
  projectId: "corcomm-blacklist",
  storageBucket: "corcomm-blacklist.firebasestorage.app",
  messagingSenderId: "…",
  appId: "1:…:web:…"
};
```

Keep the quotes and commas exactly as they are. These values aren't secret; the security rules are what protect the data.

You can find them again later under **Project settings (gear icon) → General → Your apps**.

---

## Part B – GitHub Pages (about 5 minutes)

### 7. Create the repository and upload
1. Sign in at <https://github.com> and click **New repository** (the **+** at the top right).
2. Name: `corcomm-blacklist`. Set it to **Public**, because GitHub Pages is free only for public repositories. The code is public, but no data is: everything sits behind the login and the Firebase rules.
3. Click **Create repository**.
4. Click **uploading an existing file**. Drag in `index.html`, `firebase-config.js`, `firestore.rules`, `storage.rules` and `README.md`, then click **Commit changes**.

### 8. Switch on GitHub Pages
1. In the repository, go to **Settings → Pages**.
2. Under **Build and deployment**, set **Source** to **Deploy from a branch**, **Branch** to `main`, and the folder to `/ (root)`. Click **Save**.
3. After a minute or two, the address appears at the top of that page, for example `https://YOUR-USERNAME.github.io/corcomm-blacklist/`.

### 9. Allow that address in Firebase
In Firebase: **Authentication → Settings → Authorized domains → Add domain**, and enter `YOUR-USERNAME.github.io`. This keeps the password-reset emails working with your site.

---

## Part C – First use

### 10. Create the first administrator
Open the GitHub Pages address. The first screen asks you to **set up the first administrator**. Enter your name, email and a password. This screen appears only once.

### 11. Add the team
Go to **Team → Add team user**. Enter each person's name, email address, role and a starting password, and give them the address and their starting password. They can change their password under **My account**. Anyone can use **Forgot your password?** on the sign-in screen to get a reset link by email.

To stop someone's access, open **Team → Edit** and untick **Can sign in**. To delete a login completely, use Firebase → **Authentication → Users**.

---

## Updating the app later
If you receive a new `index.html`, open the repository on GitHub, click **Add file → Upload files**, drop in the new file and commit. **Don't** upload a blank `firebase-config.js` over your filled-in one.

## Good to know
- **Logins expire:** each sign-in lasts until the browser is closed, which suits shared office PCs.
- **Server-set dates:** the transaction date on a handover and the date a payment was captured are set by Firebase's server clock. Nobody can change them, not even an administrator.
- **Protected log:** payments can't be edited, and the activity log can't be edited or deleted.
- **Document links:** opening a document creates a private link. Anyone who is given that exact link can open the file, so don't forward document links outside the company.
- **Data backups:** Firebase → Firestore → **Disaster recovery / Backups** can schedule automatic backups on the Blaze plan.
