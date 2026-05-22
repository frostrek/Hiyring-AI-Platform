# Recruiter Pipeline Dashboard

<div align="center">
  <p><b>Command Center for Recruitment Teams</b></p>
</div>

---

## Pipeline Management
The Recruiter Dashboard provides a bird's-eye view of all active job postings and their respective candidate pipelines. Candidates flow automatically from "Applied" to "Interview Completed" once their asynchronous videos are processed.

## Candidate Deep Dive
Clicking on a candidate opens their detailed evaluation card:
- **Aggregated AI Scores:** Instantly see the candidate's average score across all questions.
- **Skill Breakdown:** Radar charts showing proficiency in *Problem Solving, Communication, and Depth of Experience*.
- **Authenticity Alerts:** Prominent warnings if the AI Evaluator detected high likelihoods of scripted or heavily AI-assisted answers.

## Secure Video Playback
Raw video URLs are never exposed. When a recruiter clicks "Play" on an interview response:
- The Node.js backend dynamically generates an **AWS CloudFront Signed URL**.
- The URL is bound by a strict **15-minute expiration window**.
- The video streams directly via Edge nodes for low-latency, high-resolution playback globally, ensuring candidate data remains entirely secure and non-shareable outside the platform.
