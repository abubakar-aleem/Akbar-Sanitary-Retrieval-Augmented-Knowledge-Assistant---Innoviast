# Akbar & Sons Knowledge Assistant — RAG Chatbot

**InnoViast Week 4 — Assignment 4: Retrieval-Augmented Knowledge Assistant**
Track 03 · AI Solutions Engineering

> Live demo: https://abubakar-aleem.github.io/Akbar-Sanitary-Retrieval-Augmented-Knowledge-Assistant---Innoviast/
> GitHub repo: https://github.com/abubakar-aleem/Akbar-Sanitary-Retrieval-Augmented-Knowledge-Assistant---Innoviast

---

## Overview

A Retrieval-Augmented Generation (RAG) chatbot built for **Akbar & Sons Sanitary Store** (Lahore) that answers customer questions using only the store's product catalogue. Answers are grounded in a 27-section knowledge base — the customer never gets a made-up brand, size, or price. If a question isn't covered by the catalogue, the assistant says so and points the customer to the store's contact number instead of guessing.

## Problem Statement

Sanitary/hardware store customers often ask the same repetitive questions — "do you sell X," "what sizes/brands do you have," "what are your hours" — that staff have to answer manually every time. A single unstructured PDF or webpage catalogue isn't easily searchable by non-technical customers. This project builds a chat interface that lets customers ask in plain language and get accurate, source-backed answers pulled directly from the store's catalogue, with a clear fallback when the answer genuinely isn't in stock data.

## Features

- **Built-in knowledge base** — 27 product categories (pipes, valves, tanks, sinks, showers, ladders, locks, filters, adhesives, and more) plus store info (hours, address, contact).
- **Upload your own documents** — drag-and-drop PDF catalogues/price sheets; text is extracted and chunked entirely in-browser (via pdf.js), then merged into the knowledge base.
- **Semantic retrieval** — every chunk is embedded with Gemini's embedding model; questions are matched by cosine similarity against the vector store, not keyword search.
- **Grounded generation** — the top-3 most relevant chunks are passed to Gemini as context, with an explicit instruction not to invent facts outside that context.
- **Source transparency** — every answer displays which catalogue section(s) it drew from, with a similarity score.
- **Confidence-gated fallback** — if the best-matching chunk falls below a similarity threshold (0.55), the bot skips generation entirely and returns a fallback message with the store's contact number, instead of guessing.
- **Local persistence** — API key and computed vectors are cached in the browser's local storage, so re-visiting the page doesn't require re-embedding.

## Tech Stack

| Layer | Technology |
|---|---|
| Frontend | Vanilla HTML/CSS/JavaScript (single file) |
| PDF parsing | [pdf.js](https://mozilla.github.io/pdf.js/) (client-side, no server upload) |
| Embeddings | Google Gemini `gemini-embedding-001` |
| Generation | Google Gemini `gemini-2.5-flash-lite` |
| Retrieval | Cosine similarity, computed client-side (`TOP_K = 3`, `SIM_THRESHOLD = 0.55`) |
| Storage | Browser `localStorage` (API key + vector cache) |
| Hosting | GitHub Pages |

No backend server — all embedding, retrieval, and generation calls go directly from the browser to the Gemini API using the user's own key.

## Architecture

See `architecture-diagram.png` (or the diagram in this repo) for the full flow:

`Document input → text extraction → chunking → embedding → vector store → [question embedding] → top-k retrieval → similarity threshold check → generate grounded answer (or fallback) → display with sources`

## Setup / Run Locally

1. Clone the repo:
   ```bash
   git clone https://github.com/abubakar-aleem/Akbar-Sanitary-Retrieval-Augmented-Knowledge-Assistant---Innoviast.git
   cd Akbar-Sanitary-Retrieval-Augmented-Knowledge-Assistant---Innoviast
   ```
2. Open `index.html` directly in a browser (no build step, no dependencies to install) — or serve it locally:
   ```bash
   python3 -m http.server 8000
   ```
   then visit `http://localhost:8000`.
3. Get a free Gemini API key from [Google AI Studio](https://aistudio.google.com/apikey).
4. Paste the key into **"1. Connect"** and click **Save Key** (stored only in your browser's local storage).
5. *(Optional)* Upload PDF catalogues under **"2. Add Your Own Documents."**
6. Click **Initialize Knowledge Base** to embed all sections.
7. Ask questions in **"4. Ask a Question"** — try the quick-question chips to start.

## Screenshots

`<ADD 3–5 SCREENSHOTS HERE: empty state, connected + KB built, a successful answer with sources, and the fallback message>`

## Testing

See `Test_Questions_Evaluation_Sheet.xlsx` for the full set of 37 test cases (in-scope, out-of-scope, and edge cases) with expected behavior and pass/fail tracking.

## Learning Outcomes

- Implementing a full RAG pipeline (chunking → embedding → retrieval → grounded generation) from scratch without a framework like LangChain, to understand what those frameworks abstract away.
- Designing a similarity-threshold fallback so the assistant fails safely instead of hallucinating.
- Handling client-side PDF parsing and chunking with sentence-boundary awareness.
- Balancing chunk size/overlap and `TOP_K` against real product-catalogue data with overlapping categories.

## Known Limitations

- No prices in the knowledge base — the bot is expected to decline price questions rather than guess.
- `TOP_K=3` means broad, multi-topic questions (e.g. "what do I need for a bathroom renovation") may only surface one or two relevant categories.
- API key is stored in `localStorage`, so it's per-browser, not shared across devices, and should never be committed to the repo.

---
*Built for InnoViast — Build. Improve. Deploy. Present.*
