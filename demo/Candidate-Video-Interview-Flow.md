# 🎬 Candidate Video Interview Flow

<div align="center">
  <p><b>Step-by-Step Candidate Experience</b></p>
</div>

---

## Authentication & Setup
1. The candidate accesses their unique interview link.
2. The browser requests WebRTC permissions for Camera and Microphone access.
3. A brief hardware test ensures audio levels and video clarity are sufficient.

## Asynchronous Questioning
1. The AI Interviewer dynamically presents the first question based on the job role and parsed resume.
2. The candidate gets a short prep time before recording begins automatically or manually.
3. As the candidate speaks, the frontend captures a `video/webm` stream.

## Resilient Chunk Uploading
Behind the scenes, the video stream is continuously sliced into small chunks (e.g., `q1_<timestamp>.webm`).
- These chunks are piped directly to the **AWS S3 backend** in real-time.
- If the candidate's internet drops, a temporary retry cache captures the failure.
- A background `Recording Retry Worker` continuously polls and repairs the upload asynchronously, ensuring zero data loss without interrupting the candidate's flow.

## Submission & Cleanup
1. Once all questions are answered, the candidate sees a completion screen.
2. The backend finalizes the video objects, strictly enforcing the `STANDARD_IA` storage class.
3. The session is closed, and the video data is immediately queued for the AI Evaluation Engine.
