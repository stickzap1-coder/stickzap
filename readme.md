# StickZap Creator Studio

A new, mobile-first creator app project designed around the workflow requested for StickZap: trend research → story → direct video or frame-to-frame animation → local project storage → export and social upload preparation.

It is deliberately a **separate project**. Deploy it as a new Netlify site first; do not replace the existing StickZap site until you have tested it.

## What is included

- A new responsive dark creator workspace inspired by modern AI creation studios (not a copy of Google Flow's interface).
- **Story Lab:** uses the Gemini API to create an original title, hook, summary, complete script, and an editable scene plan with a chosen scene count. A local outline helper works without an AI API call.
- **Viral Radar:** uses the YouTube Data API to find recently published, short-duration comedy/video candidates and ranks them using public view count divided by age in hours. It includes source links and does not fabricate metrics.
- **Direct Video:** submits one prompt to Veo 3.1 and tracks the long-running operation until the clip is available for download.
- **Frame-to-Frame:** uses image generation for each scene's start and end frame, sends both frames to Veo 3.1, tracks generation, and stores the returned clip in the local project.
- Editable scenes, persistent browser project library using IndexedDB, JSON project backup/import, image downloads, individual MP4 clip downloads, and a browser-based WebM sequence renderer when `MediaRecorder` is supported.
- Caption/hashtag preparation and links to platform upload pages. This is an export workflow, not automatic direct publishing.
- Netlify Functions keep provider keys on the server side; keys are never asked for in frontend UI.

## What the project does not promise

- It cannot provide unlimited free AI video/image generation. Provider API access, model permissions, quota, billing, and costs vary. Google Flow consumer credits are not a free API quota for this app.
- Viral Radar does not guarantee a video will go viral. The YouTube Data API does not reliably expose the official Shorts designation or video aspect ratio. The app labels results as short-duration candidates and uses a simple `views / hours since upload` rate.
- Direct posting to every social network is not included. Each platform has its own app registration, OAuth authorization, permissions and review rules. The toolkit opens platform pages so you can upload the downloaded file yourself.
- The final in-browser sequence renderer outputs **WebM**; it does not transcode the combined movie to MP4. Original clips from Veo are downloaded as MP4. For a platform that insists on MP4, upload an original MP4 clip or convert the WebM using a trusted editor.
- The current Veo API workflow supports `9:16` and `16:9` video ratios. Square `1:1` still frames can be generated, but the app switches square frame-animation jobs to a supported vertical video ratio and displays that note.

## Run locally

The static UI and local outline tools run without a build step:

```bash
python3 -m http.server 8080
```

Then open `http://localhost:8080`. AI functions will not work on this simple static server because Netlify Functions are not running. For local serverless development, install the Netlify CLI and run `netlify dev` from this folder.

Tests:

```bash
npm test
```

## Deploy to Netlify — recommended beginner path

1. Download this ZIP and **extract it** into a folder on your computer. On Android, it is easier to unzip and upload from a computer, but Netlify Drop may work in a mobile browser depending on your device.
2. In Netlify, create a **new site** for the first test. Do not overwrite your current `stickzap-animation` site yet.
3. Deploy the extracted folder with the Netlify manual deploy workflow. The deployed folder must contain `index.html`, `app.js`, `styles.css`, `netlify.toml`, and `netlify/functions/`.
4. Open the new deployed URL. In Setup & Help, check that the function status responds.
5. Add environment variables in the new site's settings. If you plan to share the URL publicly, also set a strong `STUDIO_ACCESS_CODE` before connecting paid generation keys:
   - `GEMINI_API_KEY`: a Gemini API key that has access to the selected text, image, and Veo models.
   - `YOUTUBE_API_KEY`: optional, for live trend metrics. Create a key in Google Cloud and enable **YouTube Data API v3**.
   - `STUDIO_ACCESS_CODE`: optional shared access gate. Use a long, hard-to-guess value. Enter the same code in Setup & Help in each browser tab that should use the APIs. This is a simple shared gate, not individual user accounts.
6. Redeploy after adding variables. Keys are server-side. Do not paste them into `app.js`, `index.html`, screenshots, or a public chat.
7. Test in this order: generate a short story → generate one frame → generate one 8-second video → download it → test frame-to-frame → render/export. Check your current provider pricing before batch generation.
8. Download a project JSON backup before clearing browser data or moving to another device. Saved projects are local to the browser/site origin; they are not a cloud account.

## Environment variables

| Variable | Required for | Secret? |
|---|---|---|
| `GEMINI_API_KEY` | AI story generation, image generation, Veo video generation | Yes — server-side only |
| `YOUTUBE_API_KEY` | Viral Radar public search/statistics | Yes — server-side only |
| `STUDIO_ACCESS_CODE` | Shared gate for server-side API calls on a public site | Yes — server-side only |

The optional `STUDIO_ACCESS_CODE` is a basic shared gate, not full authentication; anyone you share the code with can use the configured APIs. A connection status of “Key present” means only that a value exists in Netlify. It does not prove the key is valid or has quota/model access.

## Project structure

```text
index.html
styles.css
app.js
manifest.webmanifest
service-worker.js
netlify.toml
netlify/functions/
  health.mjs
  story-generate.mjs
  image-generate.mjs
  veo-start.mjs
  veo-status.mjs
  veo-download.mjs
  trend-radar.mjs
icons/
```

## API / product references checked October 17, 2026

- Google Veo 3.1 Gemini API guide (8-second generation, portrait/landscape, first/last frames): https://ai.google.dev/gemini-api/docs/veo
- Gemini API pricing (Veo generation has paid API pricing; free tier availability is model-specific): https://ai.google.dev/gemini-api/docs/pricing
- Google Flow consumer credits/limits: https://support.google.com/flow/answer/16526234?hl=en
- YouTube Data API search: https://developers.google.com/youtube/v3/docs/search/list
- Netlify Functions overview: https://docs.netlify.com/build/functions/overview/

## Privacy notes

Story prompts and image/video prompts are sent to the configured provider only when you click the generation button. Trend search requests go to the YouTube Data API through the Netlify Function. Local project metadata and saved media are kept in IndexedDB on this device/browser. Exported project JSON files can become large because they can embed generated images and clips; store backups privately.


### Gemini story model

Story generation is configured to call `gemini-3.8-flash` via the Gemini API. Model access, quotas, and pricing depend on the Google project and current API terms. The app reports provider errors rather than claiming generation succeeded.
