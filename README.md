# Google Flow Plugin for Antigravity

An Antigravity plugin that teaches the agent how to operate [Google Flow](https://flow.google.com/) (`flow.google.com`) end-to-end via the built-in browser subagent—covering image generation (`0 credits` on PRO), clean character creation (`Portrait` + `Body`), full-frame location references, video shot generation (`Omni` / `Veo`), and scene timeline assembly.

## Structure

```text
google-flow-plugin/
├── plugin.json
├── README.md
└── skills/
    └── google-flow/
        └── SKILL.md
```

## Installation

### Global (all workspaces)
Copy or symlink this directory into your global Antigravity plugins folder:

```bash
cp -R . ~/.gemini/config/plugins/google-flow
```

*(Note: If you already have a standalone skill at `~/.gemini/config/skills/google-flow/`, remove it after installing the plugin so the two don't duplicate.)*

### Workspace-only
Copy the `skills/google-flow/` folder into your project's `.agents/skills/` directory.

## Key Workflows Included

- **Image Generation (`0 credits` on PRO):** `Nano Banana 2.1`, `Nano Banana Pro`, and `Nano Banana 2 Lite`, aspect ratios (`16:9`, `4:3`, `1:1`, `3:4`, `9:16`), thinking levels, batch counts (`x1`–`x4`), and `1K` / `2K` downloads.
- **Character Creation (`Portrait` + `Create body`):** Clean neutral-lit portraits, full-body wardrobe locks linked to the portrait, and lookalike derivation via character `Ingredients`.
- **Spatial & Set Consistency:** Full-frame ultra-wide (`14mm`–`21mm`) empty interior establishing shots and `180°` reverse-angle pairs (avoiding cutaway/diorama framing bleed).
- **Video Shots & Scenes:** `Animate`, `Video` → `Frames` (`Start`/`End` keyframes for camera transitions), `Ingredients` mode, credit confirmation before generation, and `Scenes` timeline assembly/export.
- **Auth Handling:** Pauses cleanly on `accounts.google.com` so the user can sign in directly in the browser pane, then resumes the same tab.
