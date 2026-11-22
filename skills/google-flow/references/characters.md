# Character Creation (`Portrait` + `Create body`)

- **Best practice:** Generate a **clean, neutral-lit front/3/4 portrait** on an uncluttered background via `Create character` rather than promoting dark/wide scene stills (prevents lighting/framing bleed). Optional naming pattern: `Name - Clean`. For lookalikes or alternate outfits, attach the base character as a `+` `Ingredient` when generating the second character.

## Steps
1. Click **`Add media menu`** (`+` in top bar) -> **`Create character`** (or **`Characters`** in the left `Project navigation` sidebar at `>= 1280px`) to open `/project/<uuid>/character`.
2. **From existing image:** Click **`Add from project`** (or **`Upload`**) -> select image -> **`Add media`**. Flow promotes it to the character's **Portrait** at `/character/<uuid>`; fill **`Character name`** and **`Character personality`** -> **`Done`**.
3. **From text prompt:** Fill `[aria-label="Describe your character…"]` (or pick an archetype), choose **`Select model family`**, click **`Start generation`**, wait for loading overlays to clear, then click **`Done editing`** -> **`Done`**.
4. **Full-body/wardrobe lock (`Create body`):** Beneath the `Portrait` canvas, click **`Create body`** (auto-attaches `Portrait` as an `Ingredient`), fill **`"Describe body and outfit…"`**, click **`Start generation`**, wait for completion, and click **`Done`**.
5. Saved characters appear under `+` -> **`Characters`** as reusable `Ingredient` chips across image and video prompts.
