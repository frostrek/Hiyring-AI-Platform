# 🤖 AI Evaluation & Scoring Engine

<div align="center">
  <p><b>The 4-Agent LangChain Workflow</b></p>
</div>

---

## 🧠 System Architecture

The Hiyring AI engine utilizes a robust `LangChain` architecture, primarily driven by **Gemini** with an automatic **OpenAI fallback** mechanism for maximum reliability.

### The 4 AI Agents

1. **📄 Resume Parser:** 
   - An elite intelligence engine that extracts structured data from ANY resume format.
   - Normalizes text, matches against job requirements, and calculates an initial ATS match score.
   
2. **🎤 AI Interviewer:** 
   - Uses the parsed resume and job description to formulate dynamic, targeted questions.
   
3. **⚖️ Answer Evaluator:** 
   - Analyzes transcribed candidate answers against the required skills.
   - Evaluates on 6 dimensions: *Relevance, Depth, Clarity, Communication, Problem Solving, and Practical Experience*.
   - Includes a strict **Authenticity/Plagiarism Detection** module that penalizes scripted, AI-generated, or copied answers based on word repetition, sentence length, and AI-phrase density.

4. **📝 Summary Generator:** 
   - Aggregates the dimensional scores and evaluator notes into a concise, actionable summary for the recruiter.

### 🛡️ Bias Reduction
The engine utilizes a `GENDERED_LANGUAGE_REGEX` layer. Before evaluations are processed, all gendered pronouns (he/she, him/her, Mr/Ms) and biased terms (businessman/chairman) are actively neutralized or anonymized to ensure objective scoring based purely on merit and content.
