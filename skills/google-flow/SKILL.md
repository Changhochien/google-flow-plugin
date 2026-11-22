---
name: google-flow
description: >-
  Operate Google Flow (flow.google.com) via orca-computer-use (desktop browser
  such as Safari/Chrome) or the browser subagent to generate, edit, and download
  images, create Portrait/Body characters, animate video shots, assemble scenes,
  and run Flow Tools (Nano Banana, Omni, Veo, DepthWarp 4D, Scout360).
---

# Google Flow Skill

Drive `https://flow.google.com/` via **`orca-computer-use`** in an external desktop browser (e.g., **Safari**) when available or when no `browser` subagent exists; otherwise use the **`browser` subagent** (`Model: "flash"`).

## Core Rules
- **Driver & safety:** Never use Computer Use or scripts to solve or bypass a visible CAPTCHA / human-verification popup—stop and ask the user to complete it directly.
- **Localized UI:** Google Flow labels localize to the user's Google account language (e.g., `你想製作什麼內容？`, `開始生成`, `設定觸發條件`, `下載媒體`, `要修改哪些地方？`, `Flow 代理`, `在提示詞輸入框新增素材`). Match controls by role/structure from the accessibility tree or DOM instead of assuming literal English labels.
- **Credit safety:** Check the credit cost in `Settings trigger` (or tool footer) before clicking `Start generation`. If cost > `0 credits`, confirm model, count, and cost with the user first.
- **Model drift:** If a model label changed (e.g., `Nano Banana 2.1`), pick the closest matching model family and report the actual label used.
- **Auth & local uploads:**
  - *Desktop Computer Use (macOS):* Copy local PNGs to the clipboard (`osascript -e 'set the clipboard to (read (POSIX file "<path>") as «class PNGf»)'`), focus the prompt input, and press `Cmd+V`. Flow uploads the image straight into the project and attaches it as an Ingredient in the background without opening a native file picker. On the first upload, confirm Flow's one-time image-rights notice (`I Agree` / `我同意`). When pasting multiple Ingredients, wait for each chip to appear before pasting the next so `[Image 1, Image 2]` order stays deterministic.
  - *Browser subagent:* Pass exact selectors and wait conditions in the prompt, batch DOM calls in script steps, set desktop viewport (`>= 1280px`), reuse the same subagent via `send_message`, and stage uploads in `browser_sandbox/uploads/<filename>` **after** sign-in succeeds. If `setInputFiles` hits `NAVIGATION_NEEDS_PERMISSION`, ask the user to click `Upload media` manually or attach an in-project asset.

## Image Generation, Editing & Download

### 1. Open or Create a Project
- Click **`New project`** (`/project/<uuid>`) or **`Open project`** on an existing card (or navigate directly to `/project/<uuid>`).
- Ensure the inline **`Flow Agent`** (`Flow 代理`) toggle in the bottom prompt bar is **OFF** (`Value: 0`) before direct Image generation (when ON, the bar switches to `"Follow-up"` agent chat mode and hides `Settings trigger`). If a right-hand Agent panel (`"Untitled session"`) hides the prompt bar, close it.

### 2. Configure Settings (`Settings trigger`)
- Select Mode **`Image`**, pick the model family (`🍌 Nano Banana 2.1`, `Pro`, or `2 Lite`), and set Aspect ratio, `Thinking level` *(Nano Banana 2.1 only)*, and Output count (`x1`–`x4`). Confirm displayed cost is `0 credits` (or user-approved).

### 3. Attach Ingredients (Optional)
- **Local file:** Paste via `Cmd+V` into the prompt bar (desktop Computer Use) or click `+` (`Add ingredients to the prompt box`) -> **`Upload media`** (browser subagent).
- **Existing asset:** Click `+` (or right-click tile -> **`Add to prompt`**), filter by project/category, and add the asset.
- **Multi-ingredient prompt bar vs. `/edit/`:** Use the **project grid prompt bar** whenever combining multiple Ingredients (e.g., a master reference + a layout/pose guide)—it keeps the original tiles untouched, outputs `x 2`–`x 4` separate candidates, and preserves chip order (`first reference image`, `second reference image`).

### 4. Generate & Wait
1. Fill the prompt input (`What do you want to create?` / `你想製作什麼內容？`; with `orca computer`, prefer `set-value --value-stdin`).
2. Click **`Start generation`** (`開始生成`; in Electron/Chromium webviews where `AXPress` is ignored, use verified window-local coordinates from a fresh screenshot).
3. Wait until no `^\d+%$` progress overlays remain and the new grid tiles decode cleanly (use `schedule` / `orca computer wait` or a browser script wait loop—never shell `sleep`).
4. If generation fails or is policy-blocked, report the exact error/toast instead of retrying blindly.

### 5. Edit an Existing Image (`/project/<uuid>/edit/<image-uuid>`)
- Click a tile -> fill `What do you want to change?` (`要修改哪些地方？`) -> **`Start generation`**. This implicitly conditions on the open tile and stacks versions inside that tile's history (best for quick single-image edits/variations; use the grid prompt bar for multi-ingredient runs).
- Wait for the read-only preview image to update (editor uses a shimmer/`%` state in history). Use **`Show/hide history`**, **`Done editing`**, or **`Back`**.
- **Crop:** Open the image cropping menu -> choose preset/freeform -> **`Apply crop`** or **`Cancel crop`**.

### 6. Download Images
- **1K / 2K:** Open `/edit/<image-uuid>` -> click **`Download media`** (`下載媒體`, or `More options` `⋮` at narrow widths) -> **`1K Original size`** or **`2K Upscaled`** (`4K` may require an upgraded plan). Note: in macOS background Computer Use, `Download media` is a `pop up button` that requires user approval for foreground `--restore-window` (or capture the `/edit/` preview if only a working reference is needed).
- **Quick:** Right-click grid tile -> **`Download`**. Copy from browser Downloads into the workspace if requested.

## Additional Workflows (Load Only When Needed)
- **Video shots, camera motion & Scene editor:** [references/video-and-scenes.md](./references/video-and-scenes.md)
- **Character creation (`Portrait` + `Create body`):** [references/characters.md](./references/characters.md)
- **2.5D / 3D / multi-angle `Tools`:** [references/tools.md](./references/tools.md)
