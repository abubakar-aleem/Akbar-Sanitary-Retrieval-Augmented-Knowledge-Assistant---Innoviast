# AI_USAGE.md

This document discloses how AI tools were used while building the Akbar & Sons Knowledge Assistant, per the InnoViast Week 4 working rules: *"You may use AI tools for ideation, debugging, UI suggestions, and documentation, but you must understand and explain your own work."*

> The table below is a disclosure of what actually happened during the build — only you can fill in the "How AI helped" and "What I changed / decided myself" columns honestly. The technical facts (thresholds, models, storage choices) are pulled straight from the code so you don't have to re-derive them, but the process/decision columns are left for you.

## AI Tools Used

- `<e.g. Claude / ChatGPT — for what stages>`

## Where AI Was Used

| Area | How AI helped | What I changed / decided myself |
|---|---|---|
| UI design (HTML/CSS) | `<e.g. drafted the initial layout and color scheme>` | `<e.g. adjusted the navy/gold theme, chose the store's crest style>` |
| Chunking logic | `<e.g. suggested sentence-boundary-aware chunking approach>` | `<e.g. set chunk size (800 chars) and overlap, tested against catalogue text>` |
| Retrieval logic (cosine similarity, threshold) | `<...>` | `<e.g. tuned SIM_THRESHOLD to 0.55 and TOP_K to 3 after testing false-fallback / false-answer cases>` |
| Prompt for the generation step | `<...>` | `<e.g. wrote the "answer only from context" grounding instruction>` |
| Knowledge base content (27 catalogue sections) | `<...>` | `<e.g. sourced from the store's actual price list / owner interview>` |
| Debugging | `<e.g. helped trace why fallback wasn't triggering>` | `<...>` |
| Documentation (README, this file, test sheet) | `<...>` | `<...>` |

## What I Understand and Can Explain

A short list of things you should be ready to explain live to a mentor, in your own words:

- Why retrieval uses cosine similarity over embedding vectors instead of keyword search.
- What `SIM_THRESHOLD` (0.55) and `TOP_K` (3) do, and why those specific values were chosen — below 0.55 similarity, the app skips generation entirely and returns the fallback message with the store's contact number instead of guessing.
- Why the API key and vector cache live in `localStorage` instead of a backend, and the tradeoff that implies (per-browser, not shared across devices, key exposed client-side, but no server/hosting cost or backend to maintain).
- How a PDF upload gets from raw file to embedded, retrievable chunks (pdf.js extracts text → `chunkText()` splits it into ~800-character, sentence-boundary-aware, overlapping pieces → each chunk is embedded with `gemini-embedding-001` → stored as a vector alongside the built-in 27 catalogue sections).
- What happens, step by step, when a user asks a question that scores below the similarity threshold: the question is embedded, compared by cosine similarity against every stored chunk, the top-3 matches are ranked, and if the best of those 3 still scores under 0.55 the app shows the "not in our catalogue, please contact the store" fallback instead of calling the generation model.
- Why the generation prompt explicitly restricts the model (`gemini-2.5-flash-lite`) to the retrieved context — it's told to answer only from the supplied context, not invent brands, sizes, or prices, and to say plainly when something isn't in the catalogue.

## What Was NOT Used

- No AI-generated content was used for the store's actual catalogue data (`<confirm: brands, sizes, prices where applicable — this should come from the real store, not invented>`).
- `<add anything else you deliberately kept AI out of>`

## Working Rules Compliance

- No API keys, passwords, or tokens were committed to GitHub — the Gemini API key is entered by the end user at runtime and stored only in their own browser.
- Scope was kept to a single working feature set (catalogue Q&A with fallback) rather than an unfinished larger system.
