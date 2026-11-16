# Stamp Desk

Place text stamps, seals and signatures on any PDF or photo, in the browser, and download the stamped PDF.
Documents never leave the user's computer. It is a static site (no server code, no build step).

Files: `index.html` (the whole app), `config.js` (Google Drive client ID), `vercel.json` (security headers).

## Run it locally
    python3 -m http.server 8000      # then open http://localhost:8000

## Deploy to Vercel
Option A, command line (from this folder):

    npx vercel            # first time: log in, accept the defaults, framework "Other"
    npx vercel --prod     # publish to the production URL

Option B, dashboard: push this folder to a GitHub repo, then Vercel > Add New > Project > import it.
Framework preset "Other", no build command, output directory left empty.

## Turn on "save my stamps to Google Drive" (optional)
Without this, saved stamps stay in the user's browser only, and the app never mentions Google.
When it is set up, the first "Save to library" asks the user whether to connect their own Google Drive.
The app uses the narrow `drive.file` permission: it can only see the one file it creates
(`stamp-desk-library.json`), never the rest of the user's Drive. Tokens stay in memory only.

1. Go to https://console.cloud.google.com, create a project (for example "Stamp Desk").
2. APIs & Services > Library > enable **Google Drive API**.
3. APIs & Services > OAuth consent screen: user type **External**, app name "Stamp Desk",
   add the scope `.../auth/drive.file` (it is a non-sensitive scope, so Google does not need a long review).
   While the app is in "Testing", only the test users you list can connect: add your staff emails there,
   or click **Publish app** to let anyone with a Google account connect.
4. Credentials > Create credentials > **OAuth client ID** > type **Web application**.
   Under **Authorised JavaScript origins** add your site address exactly, for example
   `https://eyasulingerih.github.io` (GitHub Pages, no path) or your own domain, and `http://localhost:8000` to test locally. No redirect URI is needed.
5. Copy the client ID (ends in `.apps.googleusercontent.com`) into `config.js`, then redeploy.

## Notes
- PDF.js and pdf-lib load from cdnjs, Google sign-in loads from accounts.google.com.
- Password-protected PDFs are refused. Scanned or photographed documents work.
- A stamp is only an image. It proves nothing about who applied it.
