VIDEO LIBRARY — GITHUB PAGES + GOOGLE SHEETS + APPS SCRIPT

WHAT THIS DOES
- Lists public Google Drive video links for everyone visiting the site.
- Lets visitors add a Google Drive video link.
- Shows newest entries first.
- Requires the private administrator password to remove an entry from the shared library.
- Stores video metadata in Google Sheets; the original video remains in Google Drive.

IMPORTANT LIMITS
- Deleting removes the entry from this website's library. It does NOT delete the original file from Google Drive.
- Anyone who can access the website can submit video links. This is open submission, so a public site can receive spam. A submission password or moderation queue can be added later.
- The original Drive file must remain shared as “Anyone with the link — Viewer” and must finish processing in Drive.
- Apps Script and Google Sheets have quotas and are not a guaranteed permanent or unlimited hosting service.

PART 1 — CREATE THE GOOGLE SHEET
1. Open https://sheets.google.com and create a blank spreadsheet.
2. Name it something like “Video Library Database”.
3. Copy the spreadsheet ID from its URL. It is the long text between /d/ and /edit.
   Example: https://docs.google.com/spreadsheets/d/SPREADSHEET_ID_HERE/edit
4. Keep this spreadsheet private. Do not publish it or share it publicly.

PART 2 — CREATE THE APPS SCRIPT BACKEND
1. Open https://script.google.com and click New project.
2. Rename the project “Video Library API”.
3. Open the default Code.gs file, remove its contents, and paste the code from this folder's Code.gs.
4. Click Project Settings (gear icon).
5. Find Script properties and click Add script property.
6. Add these two properties:
   SPREADSHEET_ID   = paste the spreadsheet ID you copied
   DELETE_PASSWORD  = your private delete password
   For the first test you may use yourbuilder, but change it to a longer private password before publishing.
7. Save the script properties.
8. Return to the editor. Choose initializeLibrary from the function dropdown and click Run.
9. Google will ask for authorization. Review the permissions and allow access using the Google account that owns the spreadsheet.
10. Return to the spreadsheet. A tab called Videos should now exist with the required headings.

PART 3 — DEPLOY APPS SCRIPT AS A WEB APP
1. In Apps Script, click Deploy > New deployment.
2. Click the gear/type selector and choose Web app.
3. Description: Video Library API.
4. Execute as: Me (your Google account).
5. Who has access: Anyone (including anonymous visitors, if this wording is shown).
6. Click Deploy and complete any authorization prompt.
7. Copy the Web app URL. It normally ends in /exec. Do not use the /dev test URL.
8. If Google does not offer anonymous “Anyone” access, your account or Workspace administrator may restrict it. The public website will not work for signed-out visitors until anonymous access is allowed.

PART 4 — CONNECT THE WEBSITE
1. Open this folder's index.html in a text editor (Notepad or VS Code).
2. Search for this line:
   const API_URL = 'PASTE_YOUR_APPS_SCRIPT_WEB_APP_URL_HERE';
3. Replace the placeholder with your deployed Apps Script URL, keeping the quotation marks. Example:
   const API_URL = 'https://script.google.com/macros/s/DEPLOYMENT_ID/exec';
4. Save index.html.

PART 5 — PUBLISH ON GITHUB PAGES
1. Sign in at https://github.com.
2. Create a repository. For a simple public site, choose Public.
3. Click Add file > Upload files.
4. Upload index.html from this folder to the repository root (not inside another folder).
5. Commit the upload.
6. Open repository Settings > Pages.
7. Under Build and deployment, choose Deploy from a branch.
8. Choose branch main and folder /(root), then Save.
9. Wait for the Pages deployment to finish. Open the published URL shown in Settings > Pages.

PART 6 — TEST IT
1. Open the published website in a private/incognito window where you are not signed in to Google.
2. The library should load (it will be empty at first).
3. Upload a small test video to Google Drive.
4. In Drive, share it with General access = Anyone with the link and role = Viewer.
5. Copy its link, return to the website, click Add video, paste the link, and submit.
6. The video should appear at the top and play in the embedded player.
7. Open the same website on another device. The same video should appear there.
8. Use Delete and enter your configured delete password. The entry should disappear from every device after refresh.

IF YOU CHANGE THE APPS SCRIPT CODE LATER
- Save the code.
- Click Deploy > Manage deployments > Edit (pencil).
- Choose New version and Deploy. The /exec URL usually remains the same for an edited deployment.

TROUBLESHOOTING
- “Could not load the library”: check API_URL; make sure it ends in /exec; check deployment access is Anyone; confirm SPREADSHEET_ID is correct; confirm initializeLibrary created the Videos tab.
- Google asks visitors to sign in: the Drive file is not shared as Anyone with the link — Viewer, or the video is not yet processed.
- Video says unavailable: confirm the original file is still in Drive, the sharing permission is public Viewer, and it is a video file supported by Drive preview.
- A delete does nothing: verify DELETE_PASSWORD in Script properties and ensure the Apps Script deployment is updated after code changes.
- Duplicate video: the backend rejects a Drive file ID already in the library.

SECURITY NOTES
- Keep the Google Sheet private.
- Never put DELETE_PASSWORD in index.html.
- The backend compares the password on the server. The password is sent to the Apps Script endpoint only when a visitor chooses Delete; use HTTPS (the deployed Google URL is HTTPS).
- The public read API exposes only video titles, Drive URLs/IDs, and added dates. Do not store private or sensitive information in this sheet.
