# AI Scoring Rubric & Logic

<div align="center">
  <p><b>Hiyring Answer Evaluation Framework</b></p>
</div>

---

## Core Evaluation Dimensions

The AI Answer Evaluator strictly scores candidates across six primary dimensions. Each dimension is normalized to a 10-point scale.

1. **Relevance:** How directly the candidate addressed the specific prompt without deviating.
2. **Depth:** The level of technical or contextual detail provided.
3. **Clarity:** How logically structured and easy to understand the response was.
4. **Communication:** Articulation, professional tone, and delivery quality.
5. **Problem Solving:** Demonstrated ability to break down complex issues or formulate solutions.
6. **Practical Experience:** Evidence of real-world application versus theoretical knowledge.

---

## Length Constraints & Scaling

Scores are heavily gated by word count to prevent hallucinated evaluations on short answers:
- **< 10 words:** Cap all scores at 0-1. No substance verified.
- **< 20 words:** Cap all scores at 0-2. Substantiation is still thin.
- **< 35 words:** Concise. Scores around 0-4.
- **< 60 words:** Solid length. Scores up to 5-6.
- **< 100 words:** Strong length. Scores up to 7-8.
- **> 100 words:** Decent length. Fully evaluated on substance.

---

## Authenticity & Plagiarism Penalties

The system actively detects non-spontaneous, AI-generated, or heavily scripted answers.

**Flags Triggered By:**
- High density of formal AI phrases (*"robust solution", "leverage", "in summary"*).
- Extremely long, perfectly structured sentences (avg > 38 words/sentence).
- Zero natural filler words (*"um", "uh"*) in lengthy responses.
- Lack of personal pronouns (*"I", "we"*) in experience-based questions.

**Penalty Impact:**
If authenticity is compromised, the `authenticity_score` drops. A penalty multiplier (e.g., `0.82` or `0.72`) is applied directly to the **Depth** and **Practical Experience** scores, effectively capping them to prevent candidates from passing using generated scripts.
