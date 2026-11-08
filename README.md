# NSA POS 2026: put it online

You will do two things once: make a free Firebase database (stores your products and sales, and keeps every device in sync), then put the page on GitHub Pages (gives you the link). About 15 minutes.

You upload two files to GitHub: `index.html` and `config.js`.

## Part 1. Firebase (free plan, no card needed)

1. Go to console.firebase.google.com and sign in with your Google account.
2. **Create a project**. Name it `NSA POS 2026`. Turn Google Analytics off. Create.
3. In the left menu: **Security > Authentication > Get started > Sign-in method > Email/Password > Enable > Save**.
4. Still in Authentication, open the **Users** tab > **Add user**. Enter your email and a strong password. This is the login you will use on NSA POS 2026.
5. In the Users list, copy your **User UID** (a long mix of letters and numbers). You need it in step 7.
6. In the left menu: **Databases & Storage > Firestore > Create database**. Pick the location **asia-southeast1 (Singapore)**. Choose **Production mode**. Create.
7. Open the **Rules** tab, delete what is there, paste this (it already has your UID from step 5), then **Publish**:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    match /{document=**} {
      allow read, write: if request.auth != null
        && request.auth.uid in ['QZYuaH16CzMkCHuUUTUxM7RiPNg2'];
    }
  }
}
```

These rules mean only your login can read or write anything.

8. On the Project Overview page click **+ Add app** and pick the web icon `</>` (or go to **Settings > Project settings > General > Your apps**). Name it `NSA POS 2026`. Leave Firebase Hosting unticked. Register app.
9. Firebase shows a block called `firebaseConfig`. Choose "Use a script tag" or "Use npm", it makes no difference. Copy each value into `config.js` (open it in any text editor, or on GitHub with the pencil icon) replacing the `PASTE_...` text. Keep the quotes. In this copy of `config.js` it is already filled in for the project `nsa-pos-2026`.

## Part 2. GitHub Pages

1. On github.com click **New repository**. Name it `nsa-pos-2026` (repository names cannot have spaces or capitals). Choose **Public** (free GitHub Pages needs a public repo). Create.
2. Click **uploading an existing file**. Drag in `index.html` and `config.js`. Commit.
3. Go to **Settings > Pages**. Under Source choose **Deploy from a branch**, branch **main**, folder **/ (root)**. Save.
4. After about a minute the page shows your link: `https://YOUR-USERNAME.github.io/nsa-pos-2026/`.
5. Open it on your phone, sign in, then use your browser's **Add to Home Screen** so it opens like an app.

## First time in the app

Open the page, set your petty cash to $100, add your products with photos, and make one practice sale. Void it afterwards on the Report tab to put the stock back.

## Good to know

- **Public link, private data.** Anyone can open the link and see the sign-in page. Nobody can read or change anything without your login.
- **Photos** are stored as small copies inside the database, so you do not need Firebase Storage or a paid plan.
- **Offline.** If the internet drops while the page is open, sales are kept on the device and sync when you are back online. If you close the page while offline it may not reopen until you have a connection.
- **Add a helper.** In Firebase Authentication add a user for them, copy their UID, and add it to the list in the rules: `in ['YOUR_UID', 'THEIR_UID']`.
- **Updating the page.** Upload a new `index.html` to the repository and commit. Your data stays.

## If something goes wrong

| What you see | What to do |
|---|---|
| "Finish setup" | `config.js` still has the placeholders, or it was not uploaded next to `index.html`. |
| "That email and password do not match" | Use the user you created in step 4. |
| "Email sign-in is switched off" | Turn on Email/Password in step 3. |
| "This account cannot use this POS" | The UID in the rules does not match your user. Copy it again and publish the rules. |
| Sign-in mentions an unauthorized domain | In Authentication > Settings > Authorized domains, add `YOUR-USERNAME.github.io`. |
