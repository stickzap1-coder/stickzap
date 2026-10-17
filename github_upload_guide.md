# GitHub + Netlify upload guide (read first)

## Important
Do not upload this ZIP as a single file if you want Netlify to deploy the app from GitHub. Extract this ZIP first, then upload the *contents* of the `StickZap-Creator-Studio` folder to your repository so that `index.html`, `app.js`, `styles.css`, `netlify.toml`, and `netlify/functions/` are visible at the repository root.

## Keep the old project safe
1. In GitHub, do not delete the old files yet.
2. Download a backup of the current repository, or create a separate new repository for testing.
3. Extract this ZIP on your device.
4. Upload the extracted project files/folders to the repository. Avoid uploading API keys or `.env` files.
5. In Netlify, make sure the linked GitHub account is linked/recognized under **Manage Git contributors**. This project cannot fix a contributor restriction by itself.
6. Trigger a deploy only after the repository file list looks right.

## Netlify settings
The included `netlify.toml` defines the publish directory and functions directory. Check that the repository root contains that file and `index.html`.

Add API keys only in Netlify site environment variables, never in GitHub source files:
- `GEMINI_API_KEY` — needed for AI generation features.
- `YOUTUBE_API_KEY` — optional, for Viral Radar.
- `STUDIO_ACCESS_CODE` — optional shared access code for API endpoints.

After changing environment variables, a new deploy may be needed. API access, model permissions, billing, and quotas depend on your provider account.

## What has been checked
The included Node test suite passes 9 local tests. This does not prove that a live Netlify deployment or live Google API calls will work; verify those after deployment.
