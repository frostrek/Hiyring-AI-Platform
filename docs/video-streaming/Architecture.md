# Video Processing & Streaming Pipeline Architecture

<div align="center">
  <p><b>Hiyring AI Platform - Core Infrastructure Documentation</b></p>
</div>

---

## Architecture Overview

The Hiyring AI Video Processing Pipeline is designed for **high-volume concurrent video uploads**, ensuring uninterrupted recording for candidates, resilient storage, and low-latency playback for recruiters. 

We utilize a robust stack centered around **Node.js, AWS S3, AWS CloudFront, and Supabase** to handle asynchronous chunked video streams efficiently.

> [!NOTE]
> This architecture handles thousands of simultaneous video streams without dropping chunks, even in poor network conditions.

---

## Pipeline Workflow

```mermaid
graph TD
    subgraph Candidate Flow
        Client[Candidate Browser] -->|WebM Chunk Stream| API(Node.js Backend)
    end
    
    subgraph Storage & Resiliency
        API -->|Uploads chunk 'q1_1234.webm'| S3[(AWS S3: STANDARD_IA)]
        API -.->|Fails/Interrupted?| RetryTemp[(S3: Temp Retry Prefix)]
        RetryTemp --> Worker[Recording Retry Worker]
        Worker -->|Recovers & Uploads| S3
    end
    
    subgraph CDN & Playback
        Recruiter[Recruiter Dashboard] -->|Req. Playback| API
        API -->|Generates Signed URL| CF(AWS CloudFront)
        CF -->|Secure Stream (15m expiry)| Recruiter
    end
    
    subgraph Archival
        S3 -.->|Lifecycle Rule| Glacier[(AWS S3 Glacier)]
    end
```

---

## Core Components

### 1. Chunked Video Ingestion
Videos are recorded asynchronously in the browser and sent as `video/webm` streams. 
- The backend parses the stream and continuously pipes chunks (e.g., `q1_<timestamp>.webm`) directly to AWS S3. 
- This minimizes memory overhead on the backend and ensures that a network drop doesn't result in complete video loss.

> [!IMPORTANT]
> All primary recordings are uploaded with the `StorageClass: STANDARD_IA` (Infrequent Access) by default. This is **hardcoded and required** to ensure optimal pricing, as videos are rarely accessed after the initial AI evaluation and recruiter review.

### 2. Resiliency & The Retry Worker
Network instability during a candidate's interview is handled gracefully.
- If an S3 upload times out, the chunk is stored in a temporary prefix (`recordings/retry-temp`).
- A background process (`recordingRetryWorker.js`) polls a Supabase queue (`recording_retry_queue_table`) every 60 seconds.
- The worker claims pending jobs, attempts to re-upload the temporary chunk to its final destination, and updates the database metadata.

### 3. Secure Playback & CDN
Raw S3 URLs are **never** exposed to the frontend.
- When a recruiter accesses a candidate's profile, the backend generates an **AWS CloudFront Signed URL**.
- Signed URLs are scoped with a rigid **15-minute expiration window**.
- CloudFront ensures fast, low-latency playback globally while preventing unauthorized access or deep-linking to the video assets.

> [!TIP]
> Playback relies on `@aws-sdk/cloudfront-signer` utilizing the `CLOUDFRONT_PRIVATE_KEY` and `CLOUDFRONT_KEY_PAIR_ID`. Ensure these environment variables are rotated securely.

### 4. Archival & GDPR Compliance
- **Glacier Lifecycle:** Because files land in `STANDARD_IA` on day zero, they cleanly cascade into AWS S3 Glacier through configured bucket lifecycle rules for long-term archival.
- **GDPR Deletion:** A dedicated service allows the batch deletion of an entire session's video objects (`deleteAllSessionVideos`), mapping all S3 objects under the candidate's session prefix and wiping them synchronously.

---

## Security Posture

| Component | Security Mechanism |
| :--- | :--- |
| **Ingestion** | Authenticated session tokens validated before S3 piping. |
| **Storage** | Private bucket ACL; no public read/write access. |
| **Delivery** | CloudFront Edge encryption + Signed URLs (TTL 15 mins). |
| **Data Privacy** | Full session cascade deletion to comply with Right to be Forgotten. |

---

<div align="center">
  <p><i>Maintained by the Frostrek Core Infrastructure Team</i></p>
</div>
