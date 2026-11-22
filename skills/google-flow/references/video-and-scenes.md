# Video Shots & Scene Editor

## 1. Generate Video Shots
- **Option A — Animate an image:** Right-click a grid tile -> **`menuitem "Animate"`** (switches to **Video · Frames** with image in **`Start`**; optionally pick **`End`** or **`Swap first and last frames`** `⇄`).
- **Option B — Configure directly:** Click **`button "Settings trigger"`** -> Mode **`Video`** -> **Video type** (`Frames` for `Start`/`End` keyframes, or `Ingredients` via `+`) -> Aspect ratio (`16:9` / `9:16`), **Model** (`Omni 1.2 Flash` with `360p`/`720p` + `Length` slider, or `Veo 3.1` variants), and Output count (`x1`–`x4`).
- **Camera moves:**
  - *Controlled angle transitions (wide-to-close-up, whip-pan, arc):* Generate `Start` and `End` stills in `Image` mode first (same full-frame location + clean character portraits), then place them in `Video` -> `Frames`.
  - *Simple push-in, pull-back, tilt, orbit:* Use `Animate` (`Start` frame only) and describe camera motion in the prompt.
- **Credit check & submit:** Confirm displayed credits in `Settings trigger` with the user if > `0`, fill `"What do you want to create?"`, and click **`Start generation`**.
- **Wait & single-clip download:** Wait in script until all `^\d+%$` overlays disappear and the playable video tile appears (videos take longer than images). To download one clip directly, right-click the tile -> **`Download`** (or open it -> `More options` `⋮` with `{ force: true }` -> `Download media`).

## 2. Create or Open a Scene
- Click **`Add media menu`** (`+` in top bar) -> **`New scene`** (`/project/<uuid>/scene/<scene-uuid>`), or click **`Scenes`** in the left `Project navigation` sidebar (`>= 1280px` viewport).

## 3. Assemble, Edit & Export
1. Click **`Add clip`** (`+`) on the timeline -> pick from **`Videos`** or **`Uploads`** / **`Upload media`**.
2. Refine via **`Toggle aspect ratio`** (`16:9` / `9:16`), playback controls, or the scene prompt bar (`"Select a clip to edit"`).
3. Click **`Download scene`** in the top bar and report the saved file path(s).
