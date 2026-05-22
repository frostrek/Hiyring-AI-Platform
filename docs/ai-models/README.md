# AI Models & Architecture

This directory outlines the AI infrastructure powering the Hiyring Platform.

## Dual-Provider Redundancy
The system relies on a highly available LLM factory utilizing both Google and OpenAI:
- **Primary Provider:** `gemini-pro` (via `langchain-google-genai`).
- **Fallback Provider:** `gpt-4o` or `gpt-3.5-turbo` (via `langchain-openai`).
- **Routing:** If the primary provider encounters a rate limit or API failure during a crucial evaluation, the system automatically falls back to the secondary provider without dropping the evaluation request.

## Bias Reduction Engine
To guarantee fair evaluations, candidate inputs are pre-processed to remove gendered indicators.
- **Regex Sanitization:** A comprehensive `GENDERED_LANGUAGE_REGEX` actively detects and neutralizes terms like *he, she, his, her, Mr., Mrs., chairman, businessman*, replacing them with gender-neutral equivalents.
- **Anonymized Cache Keys:** The caching mechanisms hash the evaluation payload strictly without candidate identifiers, ensuring identical technical answers receive the exact same score regardless of who submitted them.

## Tone & Authenticity Analysis
Instead of relying solely on LLMs to judge "tone," Hiyring uses deterministic heuristics combined with AI evaluation:
- **Filler Word Density:** Measures natural speech markers (um, uh, like).
- **Sentence Length Variance:** Flags unnaturally long, unbroken sentences typical of reading from a screen.
- **Contraction & Pronoun Checks:** Validates the presence of conversational pronouns and contractions.
