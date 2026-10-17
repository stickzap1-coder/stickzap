# Existing StickZap Studio — audit and rebuild notes

Audit date: October 17, 2026

## 1. What the existing live app does

The deployed `https://stickzap-animation.netlify.app/` page describes itself as a helper that splits a written idea into three scenes, stores character/style locks and reference images in the browser, prepares start/end image and motion prompts, and plays back uploaded clips in sequence. The page itself says generation is external and states that it does not generate images or video, assemble a final export, add music, or upload to social platforms.

## 2. What the earlier source ZIPs contained

The previously available `StickZap-Studio-AI-Animation-Studio-v2.zip` contained a static PWA shell, front-end script/styles, and Netlify function files for Veo start/status/download and Gemini image generation. It had no integrated Gemini story-writing function, no YouTube trend research function, and no end-to-end multi-scene orchestration. The function files can be syntactically valid while still failing at runtime if the deployment lacks the correct API key, quota, model access, provider permissions or supported request shape. Those remote provider paths have not been validated with a live credential in this rebuild session.

## 3. What the new project changes

- Replaces the multi-step helper presentation with one creator workspace and navigation for Overview, Viral Radar, Story Lab, Direct Video, Frame-to-Frame, and Projects & Export.
- Adds a server-side Gemini story generator that returns a title, hook, summary, full script, and an editable scene plan.
- Adds a truthful local-outline option, clearly labeled as non-AI drafting.
- Adds a YouTube Data API trend lookup that shows public source links and view-rate estimates from view count and upload age. It filters to videos up to three minutes long but does not claim the API reliably identifies official Shorts.
- Integrates image generation and Veo 3.1 direct generation. Frame-to-frame sends a start frame and a final frame to the Veo endpoint.
- Adds a frame-batch and full-scene automation action, local IndexedDB project storage, JSON backup/import, clip downloads, a client-side WebM sequence recorder, captions/hashtags, and social upload-page links.
- Keeps provider secrets in Netlify Functions, validates input, restricts video download hosts, and uses explicit error messages instead of fake success results.

## 4. What still depends on setup or limits

- `GEMINI_API_KEY` is needed for AI story, image and Veo generation. A present key does not guarantee valid model access, quota or billing.
- `YOUTUBE_API_KEY` is needed for live trend metrics. Without it, the app shows a setup error rather than fabricated trend data.
- Google Veo public API video generation is not an unlimited free service. Consumer Google Flow credits are separate from this app's API connection.
- The final sequence renderer is browser-based and outputs WebM. Veo-generated MP4 clips remain separately downloadable. Browser codec and memory support may vary, especially on mobile.
- Social links launch platform pages; direct posting requires platform-specific authorization, approved developer apps and permissions. The project intentionally does not claim automatic cross-platform publishing.
- The new app has not been deployed to Netlify from this environment and its live AI integrations have not been exercised with the user's credentials. Test each function after deployment with one generation before batch use.

## 5. Local verification performed

- JavaScript syntax check completed for `app.js` and the serverless functions.
- Nine Node tests pass for health response shape, shared access-code enforcement, missing credentials, HTTP method handling, operation validation, download-host validation, and missing YouTube key behavior.
- These automated tests are local contract tests; they do not prove the Google or YouTube remote services will accept a live generation request.


Model update: the story-generation function now requests `gemini-3.8-flash`. This is a source-level change only; a successful live API request still requires valid Netlify environment configuration and eligible Google API access.
