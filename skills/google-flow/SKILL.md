---
name: google-flow
description: >-
  Operate Google Flow (flow.google.com) via the browser subagent to generate,
  edit, and download images, create Portrait/Body characters, animate video
  shots, assemble scenes, and run Flow Tools (Nano Banana, Omni, Veo,
  DepthWarp 4D, Scout360).
---

# Google Flow Skill

Drive `https://flow.google.com/` via the `browser` subagent (`Model: "flash"`).

## Core Rules & Browser Token Efficiency
- **Pass selectors in the prompt:** The `browser` subagent does not read this file—include exact selectors, wait conditions, and a clear completion condition when delegating.
- **Minimize browser turns & screenshots:** Instruct the subagent to batch DOM interactions in script steps, use a single script wait loop for generation overlays instead of polling with screenshots, set desktop viewport (`>= 1280px` wide so the left `Project navigation` sidebar is visible), and reuse the same subagent via `send_message` for follow-ups in the same project.
- **Auth & uploads:** Pause for user sign-in on `accounts.google.com`. Stage local uploads in `browser_sandbox/uploads/<filename>` **after** sign-in succeeds (cross-domain login redirects wipe `uploads/`).
- **Credit safety:** Check the credit cost in `Settings trigger` (or tool footer) before clicking `Start generation`. If cost > `0 credits`, confirm model, count, and cost with the user first.
- **Model drift:** If a model label changed (e.g., `Nano Banana 2.1`), pick the closest matching model family and report the actual label used.

## Image Generation, Editing & Download

### 1. Open or Create a Project
- Click **`New project`** (`/project/<uuid>`) or **`Open project`** on an existing card.
- If the right-hand Agent panel (`"Untitled session"`) hides the bottom prompt bar, click **`button "Close"`** in its header.

### 2. Configure Settings (`button "Settings trigger"`)
- Select Mode **`Image`**, click **`Select model family`** (`🍌 Nano Banana 2.1`, `Pro`, or `2 Lite`), and set Aspect ratio, `Thinking level` *(Nano Banana 2.1 only)*, and Output count (`x1`–`x4`).

### 3. Attach Ingredients (Optional)
- **Local file:** Stage file per Core Rules -> click **`button "Add ingredients to the prompt box"`** (`+`) -> **`Upload media`**.
- **Existing asset:** Click `+` (or right-click tile -> **`Add to prompt`**), filter via `Select project` / `Category navigation`, select asset -> **`Add to prompt`**.
- **Spatial consistency tip:** Avoid 2.5D/cutaway dioramas as location references (causes dollhouse framing bleed). Generate a full-frame, ultra-wide (`14mm`–`21mm`) empty interior establishing shot (plus optional `180°` reverse angle), and attach it alongside clean neutral-lit character portraits as `Ingredient` chips.

### 4. Generate & Wait
1. Fill `[contenteditable="true"][aria-label="What do you want to create?"]` (use `fill()` or `click({ force: true })` if an inner child intercepts).
2. Click **`button "Start generation"`** (use `{ force: true }` if needed).
3. Wait in script until `img[alt="Tile displaying a user's image"]` is visible **and** zero `^\d+%$` overlays (including `100%`) remain.
4. If generation fails or is policy-blocked, report the exact error/toast instead of retrying blindly.

### 5. Edit an Existing Image
- **In-place (`/project/<uuid>/edit/<image-uuid>`):** Click a tile -> fill `textbox "What do you want to change?"` (optional `+` ingredients or `Settings trigger` with `Original` aspect ratio) -> **`Start generation`**. Wait in script for `img[alt="Read-only image view"]` to render with a new `src` (editor uses a shimmer state rather than `^\d+%$` overlays). Use **`Show/hide history`**, **`Done editing`**, or **`Back button to go to previous page`**.
- **Crop:** Click **`button "Button to open image cropping menu"`** -> choose preset/freeform -> **`Apply crop`** (`Crop`) or **`Cancel crop`** (`Cancel`).
- **Grid iteration:** Right-click tile -> **`Add to prompt`** to iterate as an `Ingredient`.

### 6. Download Images
- **1K / 2K:** Open `/edit/<image-uuid>` -> at desktop width (`>= 1280px`), click **`button "Download media"`** directly; at narrower widths, click **`button "More options"`** (`⋮`) with `{ force: true }` and hover **`menuitem "Download media"`** -> click **`1K Original size`** or **`2K Upscaled`** (`4K` may require an upgraded plan).
- **Quick:** Right-click grid tile -> **`menuitem "Download"`**.
- Report the saved path (and copy from the browser downloads folder into the workspace if the user requested a local destination).

## Additional Workflows (Load Only When Needed)
- **Video shots, camera motion & Scene editor:** [references/video-and-scenes.md](./references/video-and-scenes.md)
- **Character creation (`Portrait` + `Create body`):** [references/characters.md](./references/characters.md)
- **2.5D / 3D / multi-angle `Tools`:** [references/tools.md](./references/tools.md)
