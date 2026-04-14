# YouTube MCP Server

> **Connect Claude to your YouTube channel.** Read analytics, fetch full video metadata (including unlisted/private/draft), search your uploads, and update titles/descriptions/tags — all from Claude Desktop, Claude Code, or any MCP client.

[![npm version](https://img.shields.io/npm/v/youtube-studio-mcp?style=for-the-badge&color=CB3837&logo=npm)](https://www.npmjs.com/package/youtube-studio-mcp)
[![npm downloads](https://img.shields.io/npm/dm/youtube-studio-mcp?style=for-the-badge&color=CB3837&logo=npm)](https://www.npmjs.com/package/youtube-studio-mcp)
[![GitHub Stars](https://img.shields.io/github/stars/adityaarsharma/youtube-studio-mcp?style=for-the-badge&logo=github)](https://github.com/adityaarsharma/youtube-studio-mcp)
[![License MIT](https://img.shields.io/badge/License-MIT-blue?style=for-the-badge)](LICENSE)
[![Claude MCP](https://img.shields.io/badge/Claude-MCP%20Server-FF6B35?style=for-the-badge&logo=anthropic)](https://claude.ai)
[![Node.js](https://img.shields.io/badge/Node.js-18%2B-43853D?style=for-the-badge&logo=node.js)](https://nodejs.org)

---

## What Is This?

A **Model Context Protocol (MCP) server** that connects Claude to the **YouTube Data API v3** and **YouTube Analytics API** via OAuth2.

Instead of copying data from YouTube Studio into AI tools, just ask Claude:

- *"Pull the full title, description and tags for this video"*
- *"What are my top performing videos this month?"*
- *"Update the title and tags on my latest upload"*
- *"Where is my traffic coming from?"*

Claude reads your **real private channel data** and can **update video SEO directly**.

```
Claude  →  YouTube MCP Server  →  YouTube APIs  →  Your Channel Data
(you)        (this repo)           (OAuth2)         (stays local)
```
---

## Tools Available (10 total)

### Video Metadata (Read + Write)

| Tool | What It Does |
|------|-------------|
| `get_video_details` | Full metadata for any video by ID or URL — title, full description, all tags, category, privacy status (public/unlisted/private/draft), stats, duration, thumbnail URL |
| `search_my_videos` | Search your own channel's videos by keyword. Returns metadata + stats for matching videos |
| `update_video_seo` | Update title, description, and/or tags on any video directly. Only changes fields you provide |

### Channel Analytics

| Tool | What It Does |
|------|-------------|
| `get_channel_overview` | Subscribers, total views, video count, channel description, creation date |
| `get_all_videos` | List all videos with stats (views, likes, comments, tags, privacy status). Sort by date or views |
| `get_analytics_over_time` | Day-by-day views, watch time, subscribers gained/lost for any date range |
| `get_top_videos_analytics` | Top performing videos ranked by views with retention %, watch time, subs gained |
| `get_audience_demographics` | Audience breakdown: top countries, device types, age groups, gender |
| `get_traffic_sources` | Where viewers come from: YouTube Search, Suggested, Browse, External, Direct |
| `analyze_and_suggest_topics` | Pulls channel + top video data for AI-powered topic analysis |

---

## Install

### Option A: npx (Zero Install — just run it)

```bash
npx youtube-studio-mcp
```

### Option B: Global Install

```bash
npm install -g youtube-studio-mcp
```

---

### Connect to Claude

#### Claude Desktop

Edit `~/Library/Application Support/Claude/claude_desktop_config.json` (Mac) or `%APPDATA%\Claude\claude_desktop_config.json` (Windows):

```json
{
  "mcpServers": {
    "youtube-analytics": {
      "command": "node",
      "args": ["/full/path/to/youtube-studio-mcp/server.js"]
    }
  }
}
```

Or if installed via npm:

```json
{
  "mcpServers": {
    "youtube-analytics": {
      "command": "npx",
      "args": ["-y", "youtube-studio-mcp"]
    }
  }
}
```

#### Claude Code (Terminal)

```bash
claude mcp add youtube-analytics node /full/path/to/youtube-studio-mcp/server.js
```

#### VS Code

Add to `.vscode/settings.json`:

```json
{
  "mcp.servers": {
    "youtube-analytics": {
      "command": "npx",
      "args": ["-y", "youtube-studio-mcp"]
    }
  }
}
```

Restart Claude. Done!

---

## Ready-to-Use Prompts

### SEO Audit (any video)
```
Get the full details for this video: [paste URL or ID]
Check what keywords it's ranking for and suggest optimized title + tags
```

### Channel Performance Report
```
Pull my channel overview, top 20 videos by watch time, 90-day analytics,
traffic sources and audience demographics. Give me a full performance report.
```

### Video Topic Research
```
Get my top 20 videos by watch time. What patterns do you see —
topics, lengths, title styles? Suggest 10 new video ideas.
```

### Update Video SEO
```
Search my videos for "elementor menu". Pull the full details.
Write an optimized title, description, and tags — then update it.
```

### Audience Deep Dive
```
Show my full audience demographics — age, gender, countries, devices.
What content style and posting schedule fits my actual audience?
```

### Underperformer Diagnosis
```
Get all my videos. Compare bottom 10 vs top 10 by views.
Why did the lower ones underperform? What would you change?
```

---

### How the Skills + MCP Work Together

```
You: "Optimize this video: youtube.com/watch?v=abc123"

Claude:
  1. youtube-seo-optimizer skill activates
  2. Calls get_video_details → pulls current title, description, tags
  3. Checks YouTube SERP for current rankings
  4. Lists PROTECTED keywords (won't remove)
  5. Writes 3 new title options + full description + 20 tags
  6. Can call update_video_seo to apply changes directly
```

The skills tell Claude **what to do**. The MCP tools give Claude **access to your data**. Together they create a complete YouTube AI workflow.

### Install Skills

**Claude Desktop / Claude Code:**
```bash
# Copy a skill to your Claude skills directory
cp skills/youtube-seo-optimizer.md ~/.claude/skills/
```

**Or install all 8:**
```bash
cp skills/youtube-*.md ~/.claude/skills/
```

---

## Files

```
youtube-studio-mcp/
├── server.js          ← MCP server (10 tools)
├── auth.js            ← Run once to link YouTube account
├── package.json       ← Dependencies + npm config
├── skills/            ← 8 AI skills for YouTube creators
├── LICENSE            ← MIT
├── credentials.json   ← YOU add this (never commit!)
└── tokens.json        ← Auto-created after auth (never commit!)
```

---

## License

[MIT](LICENSE) — free to use, modify, share.

---

## Built By

Hakan Özdemir & Claude Code
