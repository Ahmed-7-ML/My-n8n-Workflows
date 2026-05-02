# 🎬 YouTube Video Summarizer — n8n Automation Workflow

An automated n8n workflow that accepts any YouTube URL via a web form, fetches the video transcript, generates a structured Arabic AI summary using Google Gemini, delivers it to Telegram instantly, and archives it in a Notion database — all with zero manual effort.

---

## 📌 Table of Contents

- [The Problem](#the-problem)
- [Why Automation?](#why-automation)
- [Impact of This Solution](#impact-of-this-solution)
- [What This Workflow Does](#what-this-workflow-does)
- [Workflow Overview](#workflow-overview)
- [Outputs](#outputs)
- [Tech Stack](#tech-stack)
- [Setup Guide](#setup-guide)

---

## The Problem

YouTube is the world's largest educational platform, with hundreds of hours of valuable content published daily. But consuming video content has three fundamental problems:

**Time cost.** A 30-minute video requires 30 minutes to watch — even if only 5 minutes are truly relevant. Skimming is difficult because videos have no table of contents, and titles rarely reveal the actual depth of the content inside.

**Language and accessibility barriers.** A large portion of high-quality technical content is produced in English, creating a comprehension gap for Arabic-speaking learners and professionals who want to stay current in fast-moving fields like AI, automation, and software engineering.

**No persistent knowledge store.** Even when someone watches a valuable video, the insights are rarely captured. Notes go unwritten, key ideas are forgotten, and there is no searchable record of what was learned — meaning the same video may need to be rewatched weeks later.

The result: professionals and students waste significant time either rewatching content, skipping content they should have consumed, or failing to retain what they did watch.

---

## Why Automation?

**Transcripts are structured text.** YouTube generates captions for virtually every video, making the full spoken content available as machine-readable text. There is no need to watch the video — the information is already accessible in a form that AI can process instantly.

**LLMs excel at structured summarization.** Given a transcript and a clear prompt, a model like Google Gemini can extract key ideas, organize them into a readable format, and localize the output into Arabic — in seconds, at a quality level that would take a human writer many minutes to match.

**The process is identical for every video.** The same steps apply regardless of topic, length, or channel: fetch transcript → summarize → deliver → archive. This uniformity makes it a perfect automation candidate.

**Dual delivery maximizes utility.** Sending the summary to Telegram gives instant access on any device. Saving it to Notion builds a searchable personal knowledge base that grows with every video processed — something a manual workflow would almost never sustain.

---

## Impact of This Solution

| Dimension | Before Automation | After Automation | Impact |
|---|---|---|---|
| **Time per video** | 20–60 min watching | ~30 seconds processing | 95%+ time reduction |
| **Comprehension** | Depends on focus and language ability | Structured Arabic summary always produced | Consistent quality |
| **Knowledge retention** | Mental notes, rarely written | Auto-archived in Notion with title, URL, date | Permanent and searchable |
| **Language barrier** | English content inaccessible to many | Summarized and localized in Arabic automatically | Barrier eliminated |
| **Discoverability** | Rewatching required to recall content | Search Notion database in seconds | Instant retrieval |
| **Scalability** | Each video takes the same effort to watch | Processing 10 videos = 10 form submissions | Scales at no extra cost |
| **Cost** | Time only | ~$0.001–0.01 per video (Gemini free tier) | Essentially free |

For a student or professional processing 3–5 educational videos per week, this automation saves **2–4 hours per week** while producing a growing, searchable knowledge library.

---

## What This Workflow Does

The workflow is triggered via a web form and runs the following steps:

1. **On form submission** — A web form collects the YouTube URL from the user. Accepts any valid format: standard watch links, short links, Shorts, and embed URLs.

2. **Code in JavaScript** — Parses the submitted URL using multiple regex patterns to reliably extract the 11-character YouTube Video ID regardless of URL format. Throws a descriptive error if extraction fails.

3. **HTTP Request** — Calls the `youtube-transcript.io` API with the extracted Video ID to retrieve the full transcript text plus metadata: title, publish date, and video ID.

4. **Basic LLM Chain (Google Gemini)** — Sends the video title and transcript to Google Gemini with a structured Arabic prompt. The model returns a Markdown-formatted summary with the main title, objective, key technical insights, and a practical conclusion.

5. **Send a text message (Telegram)** — Delivers the complete AI summary to a configured Telegram chat immediately after generation.

6. **Create a database page (Notion)** — Creates a new record in the Notion database with the video title, original YouTube URL, AI summary (up to 1990 characters), and the video's original publish date.

---

## Workflow Overview

The complete n8n workflow with all nodes:

![n8n Workflow](./workflow_screenshot.png)

> **Flow:** On form submission → Code in JavaScript → HTTP Request → Basic LLM Chain (+ Google Gemini) → Send a text message + Create a database page

| Node | Type | Role |
|---|---|---|
| On form submission | Trigger | Web form to accept YouTube URL |
| Code in JavaScript | Utility | Extracts Video ID from any URL format |
| HTTP Request | Data | Fetches transcript + metadata from youtube-transcript.io |
| Basic LLM Chain | AI | Generates structured Arabic summary |
| Google Gemini Chat Model | AI Sub-node | The LLM powering the summarization |
| Send a text message | Output | Delivers summary to Telegram |
| Create a database page | Output | Archives summary and metadata in Notion |

---

## Outputs

### Telegram Summary

The AI-generated Arabic summary delivered to Telegram immediately after processing:

![Telegram Output](./telegram_screenshot.png)

Each summary follows this structured Markdown format:
- **📌 العنوان الرئيسي** — Attractive reformatted video title
- **🎯 الهدف من الفيديو** — One-line value proposition for the viewer
- **📝 النقاط الجوهرية** — Key technical insights with relevant terminology
- **💡 الخلاصة والتوصية** — Practical takeaway and recommendation

### Notion Database

Each processed video archived as a searchable record:

![Notion Database](./notion_screenshot.png)

Each record contains the video title, original YouTube URL, AI-generated summary, and the video's publish date — building a fully searchable personal knowledge library over time.

---

## Tech Stack

| Component | Tool / Service |
|---|---|
| Automation platform | [n8n](https://n8n.io) |
| Input method | n8n Form Trigger |
| Transcript source | [youtube-transcript.io](https://www.youtube-transcript.io) |
| AI model | Google Gemini (via PaLM API) |
| LLM orchestration | n8n LangChain — Basic LLM Chain |
| Summary delivery | Telegram Bot API |
| Knowledge archive | [Notion](https://notion.so) Database |

---

## Setup Guide

### Prerequisites

- An n8n instance (self-hosted or [n8n.io cloud](https://n8n.io))
- A [youtube-transcript.io](https://www.youtube-transcript.io) API key
- A Google Gemini / PaLM API key ([get one here](https://aistudio.google.com))
- A Telegram bot token and your Chat ID
- A Notion integration token and a database with these properties:

| Property | Type |
|---|---|
| `Title` | Title |
| `URL` | URL |
| `Summary` | Text (Rich Text) |
| `Date` | Date |

### Steps

1. **Import** — In n8n, go to Workflows → Import → paste `YouTube_video_summarizer.json`
2. **HTTP Request node** — Replace `YOUR_YOUTUBE_TRANSCRIPT_API_KEY` in the Authorization header
3. **Google Gemini node** — Connect your PaLM API credential
4. **Telegram node** — Connect your Telegram bot and set your Chat ID
5. **Notion node** — Connect your Notion integration and set your Database ID
6. **Test** — Open the form URL, paste a YouTube link, and submit

### Customization Tips

- **Change summary language** — Edit the system message in the Basic LLM Chain node
- **Swap the AI model** — Replace the Gemini sub-node with OpenAI, Claude, or any LangChain-compatible model
- **Telegram character limit** — If summaries exceed 4096 characters, trim before sending: `$json.text.substring(0, 4000)`
- **Longer Notion summaries** — Write to the page body instead of a property to remove the 2000-character limit

---

## Upgrades to Try Next

- **Telegram trigger** — Replace the form with a Telegram bot trigger so you paste YouTube URLs directly in chat
- **Multi-language** — Add a second LLM chain to produce both Arabic and English summaries simultaneously
- **Auto-tagging** — Extract topic tags with a second LLM call and write them to a Notion multi-select property
- **Browser bookmarklet** — One-click bookmarklet that sends the current YouTube tab URL to the form automatically

---

## Project Structure

```
youtube-video-summarizer/
├── README.md
├── YouTube_video_summarizer.json   # n8n workflow export
├── workflow_screenshot.png          # n8n canvas screenshot
├── telegram_screenshot.png          # Sample Telegram summary output
└── notion_screenshot.png            # Notion database records
```

---

*Built with [n8n](https://n8n.io) — workflow automation for everyone.*
