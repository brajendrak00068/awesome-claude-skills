---
name: levea-ai-video-editor
description: Turn natural-language prompts or long recordings into finished, cloud-rendered videos with viral clips, 280+ animated caption templates, 9:16 speaker reframing, silence cleanup, B-roll, motion graphics, voiceover, and MP4 export via the Levea MCP server.
---

# Levea AI Video Editor

Autonomous cloud video editing skill powered by the [Levea MCP server](https://github.com/brajendrak00068/agentic-ai-video-production) (`levea-mcp-server` / `https://api.livecore.ai/api/v1/misc/mcp`). Describe the video outcome you want in one prompt, and Levea plans, executes, verifies, and exports a ready-to-publish MP4 without local `ffmpeg` or GPU requirements.

## When to Use This Skill

- Repurposing long podcasts, interviews, or webinars into vertical 9:16 TikTok, Reels, and YouTube Shorts clips
- Adding word-timed animated captions from 280+ built-in templates (Hormozi, MrBeast, Karaoke, Neon Pop, Subtitle Bar, Spotlight)
- Removing silences, filler words, and dead air while preserving natural speech pacing
- Reframing 16:9 horizontal footage into 9:16 vertical with automatic speaker and face tracking
- Layering B-roll, kinetic typography, motion graphics, voiceover, background music, and brand kits

## What This Skill Does

1. **Single-Call Autonomous Editing (`autonomous_edit`, `queue_edit`)**: Sends the complete user intent verbatim so the server-side planner decomposes multi-step edits (clips, captions, reframe, B-roll, music, export) in one pass.
2. **Caption & Brand Kit Management (`list_caption_templates`, `apply_caption_template`, `list_brand_kits`, `create_brand_kit`)**: Lists and applies 280+ caption styles and custom brand palettes, fonts, logos, and voices.
3. **Project, Asset & Render Lifecycle (`create_project`, `asset_upload_url`, `check_job_status`, `check_task_status`, `get_active_task`)**: Uploads source media, tracks long-running cloud renders asynchronously, and returns verified MP4 download URLs.

## Setup

Connect via the hosted Streamable HTTP MCP server (supports OAuth 2.1 + PKCE sign-in or `Authorization: Bearer <LEVEA_API_KEY>`):

```json
{
  "mcpServers": {
    "levea": {
      "url": "https://api.livecore.ai/api/v1/misc/mcp"
    }
  }
}
```

Or run via `npx`:

```json
{
  "mcpServers": {
    "levea": {
      "command": "npx",
      "args": ["-y", "levea-mcp-server"],
      "env": {
        "LEVEA_API_URL": "https://api.livecore.ai",
        "LEVEA_API_KEY": "your-levea-api-key"
      }
    }
  }
}
```

## How to Use

### Basic Usage

```
Turn this 45-minute podcast into 3 vertical 9:16 viral clips with Hormozi captions, remove silences, track the active speaker, and export MP4s for TikTok.
```

### Advanced Usage

```
Edit project 42: remove filler words and pauses over 0.6s, reframe to 9:16 with face tracking, apply my brand kit, add kinetic typography callouts on key stats, add subtle ducked background music, and export for Instagram Reels.
```

## Example

**User**: "Find the 3 best moments in my interview, reframe to 9:16 vertical, add word-by-word animated captions, and export."

**Output**:
```
1. Calls `queue_edit` (or `autonomous_edit`) with the full prompt verbatim.
2. Polls `check_task_status` / `check_job_status` until the cloud render completes.
3. Returns the verified MP4 download URLs and verification report.
```

**Inspired by:** [Levea / LiveCore AI](https://github.com/brajendrak00068/agentic-ai-video-production)

## Tips

- Send the user's complete editing goal in a single `autonomous_edit` or `queue_edit` call rather than splitting captions, reframing, and export into separate turns.
- Use `queue_edit` + `check_task_status` for full edits with motion graphics and MP4 rendering that take several minutes.
- Never re-invoke `autonomous_edit` to check progress; always poll `get_active_task`, `check_task_status`, or `check_job_status`.
- Set `requirePlanApproval: true` when the user wants to review and approve the editing plan before execution.

## Common Use Cases

- Podcast and webinar repurposing into short-form vertical clips
- Automated subtitle and animated caption generation across 60+ templates
- Faceless explainers and product launch promos with AI voiceover, B-roll, and motion graphics
