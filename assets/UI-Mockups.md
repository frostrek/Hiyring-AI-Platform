# 🖥️ UI Mockups & Design System

<div align="center">
  <p><b>Visual Architecture for the Hiyring Platform</b></p>
</div>

---

## 🎨 Candidate Portal Mockup Structure

The Candidate Portal is designed to minimize anxiety and maximize focus during asynchronous video interviews.

### Core Elements
- **Hardware Check Screen:** Validates camera and microphone permissions before the interview begins.
- **Recording Interface:** A clean viewport showing the candidate's video feed.
- **Question Prompt Area:** Displays the current interview question clearly above or alongside the video.
- **Progress Indicator:** Shows which question (e.g., Question 1 of 5) the candidate is currently answering.
- **Timer & Controls:** A countdown timer and clear "Start Recording" / "Submit Answer" buttons.

*Assets related to this view:* `Hiyring2.png`, `Hiyring3.png`

## 📊 Recruiter Dashboard Mockup Structure

The Recruiter Dashboard is a high-density, data-rich interface for pipeline management and candidate evaluation.

### Core Elements
- **Pipeline Kanban Board:** Candidates are organized into columns (e.g., "Screening", "Interviewed", "Shortlisted").
- **Candidate Profile View:**
  - **Video Playback:** Secure CloudFront-backed video player.
  - **AI Evaluation Card:** Radar charts or progress bars displaying scores for Relevance, Depth, Clarity, Communication, Problem Solving, and Practical Experience.
  - **Authenticity Warnings:** Alerts if AI-generated phrasing or extreme script-reading patterns were detected.
- **ATS Match Score:** A percentage match based on the Resume Parser's output compared to the job description.

*Assets related to this view:* `Hiyring4.png`, `Hiyring5.png`
