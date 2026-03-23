# AI Powered Cricket Highlight Generator
### Multimodal Cricket Highlight Generation System
**Submitted to:** HCL Technologies  
**Role:** Network Engineering Trainee  
**Date:** March 2026

---

## Overview

This project presents a comprehensive case study and test workbook for an 
AI-powered system that automatically generates a 10-minute cricket highlight 
reel from a full match. The system replaces manual video editing by using 
multimodal AI signals to detect, score and select the most exciting moments 
in real time.

---

## The Problem This Solves

Manual cricket highlight generation takes 3 to 5 hours per match and suffers from:

- False positives — crowd reactions and fireworks misidentified as game events
- Poor context interpretation — a boundary in a last-ball finish treated the 
  same as one in a comfortable win
- Redundant clips — same type of event repeated multiple times
- Missing narrative moments — a dropped catch that gains importance only 
  after the batsman scores a century is never included

---

## System Architecture — 4 Layers
```
Layer 1 — Data Capturing
Cameras + Microphones + Cricbuzz API + Scorecard OCR

Layer 2 — Real Time AI Processing
Computer Vision + Audio Analysis + NLP + Trajectory Detection +
Catch Difficulty Estimation + Temporal Event Linking

Layer 3 — Networking and Infrastructure
AWS Kinesis → SageMaker → S3 → CloudFront CDN → HLS Streaming
Load Balancing + SSL Encryption + WebRTC

Layer 4 — Viewer Engagement and Output
Mobile App + Web Platform + Personalised Reels + Social Sharing
```

---

## Multimodal Signals Used

The system evaluates every ball bowled using 8 independent signals:

| Signal | What it measures | Weight |
|---|---|---|
| Scoreboard | Runs, wickets confirmed via OCR | Highest |
| Context | Overs, pressure, match state | High |
| Vision | Ball, players, object detection | High |
| Trajectory | Ball arc, height, hang time | Medium |
| Catch difficulty | Fielder movement, dive, reaction time | Medium |
| Commentary | Tone, keywords, excitement level | Medium |
| Audio | Crowd intensity, decibel spike | Low — bias risk |
| Temporal impact | Future event links retroactively | Dynamic |

Audio carries the lowest weight due to crowd bias and false positive risk 
from non-game sounds like fireworks and cheerleaders.

---

## Scoring Model
```
Final Score = Event Magnitude × Context Score × Player Importance 
            × Commentary Excitement × Visual Emotion
```

Events are sorted by score. Highest scoring events fill the 10-minute 
highlight reel first. Low-score events are removed if the duration limit 
is reached.

---

## Edge Cases Handled — 12 Scenarios

| Category | Edge Case | Solution |
|---|---|---|
| False positives | Cheerleaders, fireworks detected as events | Rejected without scoreboard confirmation |
| Crowd bias | Home crowd noise exaggerates home team events | Audio normalised using stadium baseline |
| Redundancy | Same event type repeated multiple times | Events clustered — only top per cluster kept |
| Temporal | Dropped catch gains importance after batsman scores 50 | Event timeline graph — scores updated dynamically |
| Replay detection | Same six shown from three camera angles | Frame hashing deduplicates replays |
| Context conflict | Big shot in non-pressure situation | Context weighting adjusts score |
| Special cases | DRS review, Super Over | Rule-based mandatory inclusion override |
| Low scoring match | Only singles — no boundaries | Switch to bowling content and wicket sequences |
| One-sided match | Highlights dominated by one team | Forced inclusion of opposing team events |
| Silent big events | Important event with no crowd noise | Visual and scoreboard signals prioritised over audio |
| Extraordinary six | Short-distance but visually impressive shot | Distance + height + hang time evaluated separately |
| Catch difficulty | Difficult catch misidentified as routine | Pose estimation + reaction time + dive detection |

---

## Temporal Event Linking

The system does not evaluate events in isolation. It maintains a timeline 
graph that links events across the match:
```
Over 5 Ball 3  →  Dropped catch — Kohli  →  Initial score: 45 (below threshold)
                           ↓
Over 18 Ball 2  →  Kohli scores century
                           ↓
System upgrades dropped catch score retroactively to 85 — now a HIGHLIGHT
```

10 temporal linking scenarios tested — all correctly identified.

---

## Test Workbook — Excel (5 Sheets, 70 Test Scenarios)

| Sheet | Contents | Test Cases |
|---|---|---|
| Match_Events | 15 simulated ball-by-ball events | 15 |
| Highlight_Scores | 8 signal scores per event — total score calculated | 15 |
| Test_Results | Threshold decisions — pass or fail per event | 15 |
| Future_Impact_Tracking | Retroactive score upgrades — temporal linking | 10 |
| Edge_Case_Testing | All 12+ worst case scenarios tested | 15 |

**Total: 70 test scenarios — 100% pass rate**

The Excel workbook simulates what a live cricket API and sensor network 
would provide in a production system, validating the core AI logic without 
requiring real stadium hardware.

---

## Networking Layer — Key Contribution

As a network engineering professional, Layer 3 is the core technical contribution:

| Component | Technology | Purpose |
|---|---|---|
| Live ingestion | AWS Kinesis | Handles thousands of video frames per second |
| AI processing | AWS SageMaker | Runs highlight detection model at scale |
| Storage | AWS S3 | Stores all highlight clips with redundancy |
| Fast delivery | AWS CloudFront CDN | Edge servers near each viewer — under 500ms load |
| Streaming | HLS protocol | Adaptive bitrate — works on 4G and 5G |
| Scale | Auto-scaling groups | Handles 10 million viewers during IPL finals |
| Security | SSL encryption | Protects all video data in transit |
| Real-time | WebRTC | Sub-second latency for live highlight delivery |

---

## RASI Framework

| Task | Responsible | Accountable | Support | Informed |
|---|---|---|---|---|
| Video and audio capture | Hardware team | Data layer lead | Cloud engineers | Product team |
| AI highlight detection | ML model | AI lead | NLP and CV engineers | QA team |
| Network and delivery | Network engineer | DevOps lead | Cloud architects | Management |
| User interface | Frontend developer | Product owner | UX designer | Stakeholders |
| Testing and quality | QA engineer | QA lead | All teams | HCL management |

---

## Files in This Repository

| File | Description |
|---|---|
| `Cricket_Highlight_Generation_System_Case_Study.docx` | Full case study — 11 sections covering architecture, edge cases, scoring model and results |
| `Cricket_Highlight_Generator.xlsx` | 5-sheet test workbook — 70 test scenarios with 100% pass rate |

---

## Key Takeaways

- Multimodal validation prevents false positives — an event must be confirmed 
  by scoreboard data before being accepted
- Temporal event linking ensures narrative moments are never missed
- The networking layer is critical — without CDN and HLS streaming, highlights 
  cannot reach millions of viewers within seconds of occurring
- Testing with Excel simulates the full production pipeline without requiring 
  real stadium hardware, proving the logic is sound before deployment
```

