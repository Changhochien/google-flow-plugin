---
name: google-flow
description: >-
  Generate, edit, animate, and download images, video shots, and scenes using
  Google Flow (flow.google.com) via the browser subagent. Use this skill
  whenever the user asks to generate or edit images, animate images into video
  shots, or build scenes with Google Flow (Nano Banana, Omni, or Veo models).
---

# Google Flow Skill

Use this skill to drive `https://flow.google.com/` through Antigravity's `browser` subagent (`Model: "flash"`).

## Prerequisites & Auth Handling

1. Delegate to the `browser` subagent (`Model: "flash"`).
2. If the browser redirects to `accounts.google.com` or prompts for sign-in, pause and ask the user to sign in directly in the browser pane.
3. Once the user confirms sign-in, resume the **same** `browser` subagent via `send_message` (do not spawn a new one).
4. **Credit safety rule:** Image generation uses `0 credits` on the PRO tier, but **video generation costs credits**. Always check the displayed credit cost in `Settings trigger` and ask the user for confirmation before clicking `Start generation` for any video shot (unless the user already approved the model/count/cost).

## Image Generation Workflow

### 1. Open or Create a Project
- On `https://flow.google.com/`:
  - Click **`New project`** (`+ New project`) to start a fresh project (`/project/<project-uuid>`), **or** click **`Open project`** on an existing project card.
- **Important:** If the right-hand Agent panel (`"Untitled session"`) opens and hides the bottom prompt bar, click **`button "Close"`** in the Agent panel header to reveal the standard bottom prompt bar.

### 2. Configure Image Settings
Click **`button "Settings trigger"`** on the right side of the bottom prompt bar:
- **Mode:** Select **`Image`** (default).
- **Model:** Click **`Select model family`** and choose:
  - `🍌 Nano Banana 2.1` (default)
  - `🍌 Nano Banana Pro`
  - `🍌 Nano Banana 2 Lite`
- **Aspect ratio:** `16:9` (default), `4:3`, `1:1`, `3:4`, or `9:16`.
- **Thinking level** *(only visible for `Nano Banana 2.1`)*: `Low`, `Medium`, or `High` (default).
- **Output count:** `x1`, `x2` (default), `x3`, or `x4`.

### 3. (Optional) Attach Reference Images, Characters, or Ingredients
- **Best practice for spatial consistency (Full-Frame Location References):** Avoid 2.5D/isometric cutaway dioramas as location references—the model will copy the cutaway/dollhouse framing into your shots. Instead, generate a **full-frame, edge-to-edge ultra-wide (`14mm`–`21mm`) empty interior establishing shot** packed with surrounding architectural, prop, and lighting detail (`Image` mode, `0 credits`)—and optionally a **180° reverse-angle** establishing shot for full-room coverage. Then attach **both** the matching full-frame location image (`Images` tab) and your `- Clean` characters (`Characters` tab) as **`Ingredient`** chips.
- **From your local machine:** Stage each file in `<Artifact Directory Path>/browser_sandbox/uploads/<filename>`, then click **`button "Add ingredients to the prompt box"`** (`+` on the left of the prompt bar) -> **`Upload media`**.
- **From existing project Images, Characters, or other projects:**
  1. Click **`button "Add ingredients to the prompt box"`** (`+`), or right-click any project tile and choose **`Add to prompt`**.
  2. Filter by **`Select project`** and **`Category navigation`** (`All`, `Images`, `Videos`, `Characters`, `Avatars`, `Uploads`).
  3. Select the asset and click **`Add to prompt`** (attaches as an **`Ingredient`** chip in the prompt bar).

### 4. Submit Prompt & Wait for Generation
1. Focus and fill the `contenteditable="true"` textbox with `aria-label="What do you want to create?"` (if a normal click hits an inner child element, use `fill()` or `click({ force: true })`).
2. Click **`button "Start generation"`** (becomes enabled once text is entered; use `click({ force: true })` if an inner child intercepts the click).
3. Wait for the generated tile(s) (`img[alt="Tile displaying a user's image"]`) to render **and** for all percentage loading overlays (`^\d+%$`, including `100%`) to disappear so the outputs are fully revealed.

### 5. Download Images
- **Specific resolution (`1K Original size` or `2K Upscaled`):**
  1. Click a generated image tile to open `/project/<project-uuid>/edit/<image-uuid>`.
  2. Click **`button "More options"`** (`⋮`) in the top-right banner.
  3. Hover **`menuitem "Download media"`** and click **`1K Original size`** or **`2K Upscaled`** (`4K Upscaled` requires an upgraded plan if disabled).
- **Quick download:** Right-click an image tile on the project grid and click **`menuitem "Download"`**.
- Report the saved/downloaded file path(s) and generation details back to the user.

## Video Shot & Scene Workflow

### 1. Generate Video Shots
- **Option A — Animate an existing image:**
  1. Right-click an image tile in the project grid -> click **`menuitem "Animate"`**.
  2. Flow switches the bottom prompt bar to **Video · Frames** mode with that image in the **`Start`** frame slot (optionally click **`End`** to pick an ending frame from `Images`, or **`Swap first and last frames`** `⇄`).
- **Option B — Configure Video mode directly:**
  1. Click **`button "Settings trigger"`** -> select **Mode: `Video`**.
  2. Choose **Video type**: **`Frames`** (`Start`/`End` keyframes) or **`Ingredients`** (reference assets/characters via `+`).
  3. Choose **Aspect ratio** (`16:9` or `9:16`), **Model** (`Omni 1.2 Flash` with `360p`/`720p` + `Length` slider, `Veo 3.1 - Lite`, `Veo 3.1 - Fast`, or `Veo 3.1 - Quality`), and **Output count** (`x1`–`x4`).
- **Camera movement & angle changes in video:**
  - For **controlled angle transitions** (e.g., wide-to-close-up, whip-pan, or arc), generate both the **Start** and **End** angle stills first (`Image` mode, `0 credits`) using the same Full-Frame Location + `- Clean` Characters, then put them in `Start` and `End` under **`Video` → `Frames`** and describe the camera move.
  - For **simple push-ins, pull-backs, tilts, or orbits**, use `Animate` (only `Start` frame) and describe the camera motion in the prompt.
- **Confirm credits** shown in the Settings footer with the user, then fill `"What do you want to create?"` and click **`Start generation`**.

### 2. Create or Open a Scene
- Click **`Add media menu`** (`+` in the top header) -> **`New scene`** (navigates to `/project/<project-uuid>/scene/<scene-uuid>`), **or** select **`Scenes`** in the left `Project navigation` sidebar to open an existing scene.

### 3. Assemble, Edit & Export Shots in the Scene Editor
1. Click **`Add clip`** (`+`) on the timeline -> select clips from **`Videos`** (or **`Uploads`** / **`Upload media`**).
2. Use **`Toggle aspect ratio`** (`16:9` / `9:16`), playback controls, or the scene bottom prompt bar (`"Select a clip to edit"`) to refine shots.
3. Click **`Download scene`** in the top bar to export the finished scene, and report the saved file path(s) to the user.

## Character Creation Workflow

- **Best practice:** Generate a **clean, neutral-lit front/3/4 portrait** on an uncluttered background via `Create character` rather than promoting dark or wide scene stills (which causes lighting/framing bleed). For lookalikes or alternate outfits, attach the base character as an `+` **`Ingredient`** when generating the second character.

1. Click **`Add media menu`** (`+` in the top bar) -> **`Create character`** (or **`Characters`** in the left `Project navigation` sidebar) to open `/project/<uuid>/character`.
2. **From an existing image:** Click **`Add from project`** (or **`Upload`**) -> select an image -> **`Add media`**. Flow immediately promotes it into the character's **Portrait** at `/character/<uuid>`, where you fill **`Character name`** and **`Character personality`** and click **`Done`**.
3. **From a text prompt:** Fill **`aria-label="Describe your character…"`** (or pick a sample archetype), choose **`Select model family`** (`🍌 Nano Banana 2.1`, `Nano Banana Pro`, or `Nano Banana 2 Lite`), click **`Start generation`**, and click **`Done editing`** / **`Done`** to save.
4. **Add full-body/wardrobe reference (`Create body`):** After the `Portrait` exists, click **`Create body`** beneath the canvas (auto-attaches the `Portrait` as an `Ingredient`), enter **`"Describe body and outfit…"`**, click **`Start generation`**, and click **`Done`**.
5. Saved characters are available under `+` -> **`Characters`** as reusable **`Ingredient`** chips across any image or video prompt.

## 2.5D, Multi-Angle & Scene Exploration (`Tools`)

1. Open **`Tools`** in the left `Project navigation` sidebar (`/project/<uuid>/tools`) and browse **`Templates`** or **`Community`** (or build a custom one via **`Create tool`**, which currently costs `0` credits):
   - **2.5D / 3D & Camera:** `DepthWarp 4D` (2.5D depth-warp), `3D Model Visualizer`, `Scout360` (360° environment), `Frame Deconstructor` (3D sculptures), `Director Camera & Path Vector Board`.
   - **Scene & Shot Exploration:** `Scene Explorer` (explore scene visuals from a location), `Shot Explorer` (new angles of your scene), `Storyboard Studio`, `Mockup`, `LuzRelighting`.
2. Open the desired tool, feed in a project image or prompt, run it, and save/use the output as an **`Ingredient`** in your project.
