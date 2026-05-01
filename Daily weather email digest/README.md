# 🌤 Daily Weather Email Digest — n8n Automation Workflow

An automated n8n workflow that fetches the daily weather forecast for your city every morning and delivers a clean, formatted email digest straight to your inbox — no manual effort required.

---

## 📌 Table of Contents

- [The Problem](#the-problem)
- [Why Automation?](#why-automation)
- [Impact of This Solution](#impact-of-this-solution)
- [What This Workflow Does](#what-this-workflow-does)
- [Workflow Overview](#workflow-overview)
- [Email Output](#email-output)
- [Tech Stack](#tech-stack)
- [Setup Guide](#setup-guide)
- [Workflow JSON](#workflow-json)

---

## The Problem

Every morning, millions of people waste time doing the same repetitive task: opening a weather app, reading conditions, and deciding what to wear or how to plan their day. For individuals this is a small nuisance — but multiply it across a team, a fleet of field workers, or an operations department, and you have a real drain on productive time.

More specifically:

- **Field workers and delivery drivers** need weather briefings before starting their shifts — checking manually introduces delays and inconsistency.
- **Event planners and operations managers** need early weather awareness to make last-minute decisions, but often forget to check until it's too late.
- **Remote teams across time zones** have no single source of truth for daily conditions in each team member's city.
- **Anyone with a busy morning routine** loses context-switching time toggling between apps before starting deep work.

There is no need for a human to perform this lookup — the data is publicly available via API, the format is always the same, and the delivery channel (email) is already part of everyone's morning routine.

---

## Why Automation?

Automation is the right solution here for several reasons:

**The task is 100% rule-based.** Every step is deterministic: check the clock, call an API, format the result, send an email. There is no judgment or creativity required — which makes it a perfect candidate for automation.

**The trigger is time-based.** The workflow needs to run at the same time every day regardless of whether anyone is at a computer. A scheduled automation handles this naturally; a human cannot.

**The output is always the same shape.** Weather data from an API returns a consistent JSON structure. Mapping it to an email template is a one-time setup that runs forever without maintenance.

**It removes cognitive load.** By delivering the information proactively to where the user already is (their inbox), the user never has to remember to check anything.

**It scales effortlessly.** Whether you want weather for 1 city or 20, the workflow handles it — a human doing this manually scales linearly with effort; automation does not.

---

## Impact of This Solution

| Dimension | Before Automation | After Automation | Impact |
|---|---|---|---|
| **Time** | 2–3 min/day checking weather apps | 0 min — delivered automatically | Saves ~15 hours/year per person |
| **Effort** | Manual, requires remembering | Fully passive | Zero cognitive load |
| **Consistency** | Depends on person's habit | Runs every day at 8 AM, always | 100% reliable delivery |
| **Scalability** | Linear with team size | Flat — same effort for 1 or 100 | Scales at no extra cost |
| **Cost** | Free (just your time) | ~$0/month (free API tier + n8n) | No added cost |
| **Decision Speed** | Weather checked reactively | Weather in inbox before the day starts | Faster morning planning |
| **Missed Information** | Easy to forget on busy mornings | Never missed | Reduces operational blind spots |

For a team of 10 field workers, this automation saves approximately **150+ hours per year** in aggregate — time that can be redirected to actual work.

---

## What This Workflow Does

The workflow runs automatically every morning at **8:00 AM** and performs the following steps:

1. **Triggers on schedule** — A Schedule Trigger node fires at 8 AM daily with no human intervention.
2. **Fetches live weather data** — An HTTP Request node calls the WeatherAPI to retrieve current conditions including temperature, feels-like, humidity, wind speed, and a text description of conditions.
3. **Formats the message** — An Edit Fields (Set) node maps the raw API response into a clean, human-readable email body with contextual tips (e.g., "Hot day ahead — stay hydrated!" if temp > 30°C).
4. **Sends the email** — A Gmail node delivers the formatted message to your inbox automatically.

---

## Workflow Overview

The complete n8n workflow with all four connected nodes:

![n8n Workflow](./workflow_screenshot.png)

> **Nodes:** Schedule Trigger → HTTP Request → Edit Fields → Send a message (Gmail)

---

## Email Output

A sample of the delivered email each morning:

![Email Output](./email_screenshot.png)

The email includes:
- Current temperature and feels-like temperature (°C)
- Weather condition description (e.g., Sunny, Partly Cloudy)
- Humidity percentage
- Wind speed
- A contextual tip based on temperature range

---

## Tech Stack

| Component | Tool / Service |
|---|---|
| Automation platform | [n8n](https://n8n.io) |
| Weather data | [WeatherAPI.com](https://www.weatherapi.com) |
| Email delivery | Gmail (via n8n OAuth2) |
| Trigger | n8n Schedule Trigger (daily at 8 AM) |

---

## Setup Guide

### Prerequisites

- An n8n instance (self-hosted or [n8n.io cloud](https://n8n.io))
- A free API key from [weatherapi.com](https://www.weatherapi.com)
- A Gmail account connected to n8n via OAuth2

### Steps

1. **Import the workflow** — In n8n, go to Workflows → Import → paste the JSON from `Daily_weather_email_digest.json`
2. **Set your city** — In the HTTP Request node, change the `q` query parameter to your city name
3. **Add your API key** — In the HTTP Request node, replace the `key` parameter value with your WeatherAPI key
4. **Connect Gmail** — In the "Send a message" node, connect your Gmail account via OAuth2 and set your recipient email address
5. **Activate** — Toggle the workflow to **Active** and it will run automatically every morning at 8 AM

### Customization

- **Change the time** — Edit the Schedule Trigger node to fire at a different hour
- **Change the city** — Update the `q` parameter in the HTTP Request node
- **Change the email recipient** — Update the `sendTo` field in the Gmail node
- **Add more cities** — Duplicate the HTTP Request → Edit Fields path and add a Merge node before Gmail

---

## Workflow JSON

The full workflow definition is in [`Daily_weather_email_digest.json`](./Daily_weather_email_digest.json). Import it directly into n8n to get started instantly.

---

## Project Structure

```
daily-weather-email-digest/
├── README.md
├── Daily_weather_email_digest.json   # n8n workflow export
├── workflow_screenshot.png           # n8n canvas screenshot
└── email_screenshot.png              # Sample email output
```

---

*Built with [n8n](https://n8n.io) — workflow automation for everyone.*
