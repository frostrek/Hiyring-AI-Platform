# API Reference & ATS Integration

This directory contains integration guides for hooking Hiyring into existing Applicant Tracking Systems (ATS).

## Integration Philosophy
Hiyring acts as the intelligent middleware between the top of your funnel (ATS Application) and the human interview stage.

### Common Webhook Flow
1. **Candidate Applies (ATS):** Webhook triggers Hiyring's `POST /api/v1/candidates/sync`.
2. **Resume Parsing:** Hiyring's Resume Parser agent ingests the raw PDF/Word document, extracts structured JSON, and calculates an initial Job Match Score.
3. **Interview Invitation:** Hiyring automatically emails the candidate a unique video interview link.
4. **Evaluation Completion:** Once the video processing pipeline and AI Evaluation Engine finish, Hiyring pushes a webhook back to the ATS.
5. **Data Sync (ATS):** The ATS is updated with the candidate's average score, the AI-generated summary, and a secure CloudFront link for video playback.

## Key Endpoints (Internal)
- `POST /api/v1/interviews/sessions` - Initializes a new candidate session.
- `PUT /api/v1/interviews/chunk` - Accepts chunked WebM video uploads.
- `GET /api/v1/interviews/playback/:s3Key` - Generates a secure, 15-minute CloudFront signed URL for video streaming.
- `DELETE /api/v1/interviews/gdpr-purge` - Batch deletes all session videos to comply with GDPR Right to be Forgotten.

*For detailed OpenAPI specifications, please contact the Frostrek integration team.*
