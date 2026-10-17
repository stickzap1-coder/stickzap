# StickZap Creator Studio — quick start for beginners

## 1. Start a project
Tap **New project**, open **Story lab**, then type one short idea. Example: “A sleepy student tries to sneak a snack into class, but every wrapper makes a louder noise.” Choose a genre, language, and number of scenes.

## 2. Generate the story
Tap **Generate story & scenes** if `GEMINI_API_KEY` is connected. If you have not connected it, tap **Quick local outline** to create an editable starter outline locally. A local outline is not AI generation.

## 3. Create one direct video
Open **Direct video**. Tap **Import current story**, check the prompt, choose 9:16 for vertical Shorts, and tap **Generate video clip**. Leave the page open while the provider works. When complete, preview and download the MP4.

## 4. Create scene-by-scene video
Open **Frame-to-frame**. For each scene, generate a start frame and an end frame. Review both images. Then tap **Animate scene**. Generate one scene at a time before trying batches because every image/video request can use API quota or credits.

## 5. Render the combined sequence
Once you have scene clips, open **Projects & export** and tap **Render final sequence (WebM)**. Keep the page open. The browser plays your saved clips in order and records their picture plus available audio. Download the result when it is ready. If your target platform requires MP4, use a trusted video editor to convert WebM or upload an original downloaded MP4 clip.

## 6. Prepare your post
Add a caption and hashtags, then tap **Copy caption & hashtags**. Download your video, open the platform link, and upload it yourself. Direct auto-posting is not connected in this version.

## 7. Save your work
The app auto-saves to the current browser. Use **Export project JSON** to back up story text, scenes, images and clips. Keep that JSON file safe. Use **Import project JSON** on another browser to restore it.

## If something fails
- “Not connected”: add the environment variable in Netlify and redeploy.
- “Quota/billing/model” error: check Gemini API/Google Cloud settings and current pricing.
- Trend search cannot load: add `YOUTUBE_API_KEY`, enable YouTube Data API v3, and redeploy.
- Video still processing: wait a little, then retry. Do not click generation repeatedly, because each successful request may create a separate charge or consume credits.
- App page works but API routes are 404: deploy the complete extracted folder, including `netlify.toml` and `netlify/functions/`.


**Story model:** The current source calls `gemini-3.8-flash`. If Google reports a model-access or quota error, check the official Gemini API model list and your project access.
