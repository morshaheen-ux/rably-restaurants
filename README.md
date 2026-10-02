# Rably — Restaurant tracker

A single-page app (Arabic / English) for working through Rably's restaurant prospect list. You can:

- **Track every restaurant's status:** Not started, In progress, Done, Cancelled or Rejected.
- **Assign restaurants to team members**, one at a time or in bulk.
- **Add restaurants by hand**, edit their details and keep notes. Every change goes into a history log.
- **Import the Excel export again later, with more rows.** New restaurants are added and existing ones get updated details. Assignment, status and notes are never overwritten.
- **Export** the current filtered view to Excel, and see every restaurant on a map coloured by status.

Data lives in **Firebase Firestore**. The site is hosted on **GitHub Pages**.

## One-time setup

### 1. Firebase
1. Go to [console.firebase.google.com](https://console.firebase.google.com). Create a project or use an existing one.
2. Go to **Build → Firestore Database → Create database**. Production mode is fine.
3. Go to **Firestore → Rules**. Paste in the contents of `firestore.rules` and click **Publish**.
4. Go to **Build → Authentication → Get started → Email/Password**, enable it, then open **Users → Add user** and add an account for each team member.
5. Go to **Project settings → Your apps → Web (`</>`)** and register an app. Copy the `firebaseConfig` values into `firebase-config.js`.
6. Go to **Authentication → Settings → Authorized domains** and add `<your-github-user>.github.io`.

### 2. GitHub Pages
In the repo, go to **Settings → Pages → Build and deployment**. Set the source to **Deploy from a branch** and pick `main` with the `/ (root)` folder.
The site will appear at `https://<user>.github.io/<repo>/`.

### 3. Load the restaurants
Sign in. The empty screen offers **Load starter list**, which imports the 897 restaurants in `data/restaurants.json`. Alternatively, choose **Import Excel** and pick the original file.

## Importing more data later
Use the same column layout as the Google Maps export: `number, name, rating, reviews, phone, international_phone, website, address, latitude, longitude, category, types, price, status, google_maps_url, place_id, opening_hours`. Only `name` is required.

- **Matching:** rows are matched by `place_id`. If a row has no `place_id`, it's matched by name and phone.
- **Preview:** before anything is written, you'll see how many rows are new, updated and unchanged.
- **The Excel `status` column** holds the Google business status (OPERATIONAL…). It's stored separately from the work status.

## Team
In the **Team** tab, add the people you assign restaurants to. If you enter an agent's email and it matches their login, that agent also gets a **My restaurants** filter.

## Files
| File | Purpose |
|---|---|
| `index.html` | The whole app |
| `firebase-config.js` | Your Firebase web config (not secret — security comes from Auth + rules) |
| `firestore.rules` | Only signed-in users can read/write; the history log is append-only |
| `data/restaurants.json` | Starter list (897 restaurants, Damascus) |
