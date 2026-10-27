# Esports Scoreboard: connect the backend and publish

Two pages share one free cloud database (Firebase Realtime Database):

- `index.html` is the public scoreboard. Anyone can open it, nobody can edit it.
- `admin.html` is the private admin panel. Sign in with your email and password, edit, and the scoreboard updates live.
- `config.js` holds your Firebase details. `bg.png` is the background image.

## Part 1: Create the backend (Firebase, about 5 minutes)

1. Go to https://console.firebase.google.com and sign in with a Google account.
2. Click **Create a project**, give it a name (for example `esports-scoreboard`), turn Google Analytics off, and click **Create**.
3. In the left menu open **Build → Realtime Database → Create Database**.
   - Pick any location.
   - Choose **Start in locked mode** and click **Enable**.
   - Copy the database URL at the top of the Data tab. It looks like `https://esports-scoreboard-default-rtdb.firebaseio.com` (some locations end in `.firebasedatabase.app`).
4. Open **Build → Authentication → Get started → Sign-in method → Email/Password → Enable → Save**.
5. Open the **Users** tab, click **Add user**, and enter your admin email and a strong password. This is your admin login.
6. Click the gear icon, then **Project settings → General**, and copy the **Web API Key**.
7. Go back to **Realtime Database → Rules**. Replace everything with this (use your admin email in lowercase), then click **Publish**:

```json
{
  "rules": {
    ".read": true,
    ".write": "auth != null && auth.token.email === 'your-admin-email@gmail.com'"
  }
}
```

This lets everyone read the scoreboard, but only your admin email can change it.

## Part 2: Connect the website to the backend

Open `config.js` in any text editor and paste in your two values:

```js
window.CFG = {
  databaseURL: 'https://esports-scoreboard-default-rtdb.firebaseio.com',
  apiKey: 'AIzaSy...your-web-api-key...',
  background: 'bg.png'
};
```

Keep `index.html`, `admin.html`, `config.js` and `bg.png` together in one folder.

## Part 3: Put it online (GitHub Pages)

1. On https://github.com click **New repository**, name it `esports-scoreboard`, set it to **Public**, and create it (do not add a README).
2. In Terminal, go into the folder that has your files and run:

```
cd ~/Downloads/scoreboard
git init
git add .
git commit -m "Esports scoreboard"
git branch -M main
git remote add origin https://github.com/amankumar1997aaa-dotcom/esports-scoreboard.git
git push -u origin main
```

3. On the repository page open **Settings → Pages**. Under **Build and deployment**, set **Source: Deploy from a branch**, **Branch: main**, **Folder: / (root)**, and click **Save**.
4. Wait about a minute. Your links are:
   - Scoreboard: `https://amankumar1997aaa-dotcom.github.io/esports-scoreboard/index.html`
   - Admin: `https://amankumar1997aaa-dotcom.github.io/esports-scoreboard/admin.html`

To update later, change the files and run:

```
git add .
git commit -m "Update"
git push
```

## Part 4: Test that it is connected

1. Open the admin link on your phone and sign in with the email and password from step 5.
2. Open the scoreboard link on another screen. A red **● LIVE** badge should show at the top right.
3. In the admin, open **Teams**, change a team name, and watch it change on the scoreboard within a second or two.

## Using it at the event

- Edit results, teams and points in the admin. Changes save automatically.
- The **On-screen control** buttons at the top of the admin switch the public scoreboard to a map, the Final or the MVP tab, and trigger Replay, Reveal and Celebrate.
- Add `?fs` to the scoreboard link (`index.html?fs`) to open it already in full-screen layout. This suits a projector, a TV browser or an OBS browser source. On a computer you can also click **Full screen** or press **F**.
- To bring over data from the old single-file scoreboard: in the old file go to Admin → Data → Export, then paste it into the new admin under Data → Import backup.
- Edit from one device at a time. The last save wins.

## If something goes wrong

| What you see | Fix |
| --- | --- |
| Admin says "Not connected yet" | `databaseURL` or `apiKey` in `config.js` is empty. Fill both in and re-upload. |
| "Wrong email or password" | Use the user you added in Authentication → Users. |
| "Permission denied" when saving | The email in the database rules must be your admin email in lowercase and match exactly. Publish the rules again. |
| Scoreboard shows "Demo data · not connected" | `config.js` is missing from the site or has empty values. |
| Changes don't appear on the scoreboard | Reload the page. Check the badge says LIVE, not Offline. |
| Background image missing | Keep `bg.png` next to `index.html`. A copy is also built into `index.html`. |
