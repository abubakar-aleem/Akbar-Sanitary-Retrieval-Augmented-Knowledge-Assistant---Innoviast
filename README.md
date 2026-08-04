# Akbar & Sons Knowledge Assistant — RAG Chatbot

**Week 4: Retrieval-Augmented Knowledge Assistant**  
InnoViast AI Development Track 03 — Summer Internship 2026

---

## Problem Statement

Customer support for retail businesses often relies on either:
1. **Generic chatbots** that hallucinate answers (wrong product info, made-up prices, false claims)
2. **Manual support teams** that can't scale (long wait times, inconsistent responses)

Akbar & Sons Sanitary Store needed a middle ground: an AI assistant that answers questions **only from verified product data**, cites sources, and knows when to say "I don't know—contact the store."

This project builds a **Retrieval-Augmented Generation (RAG)** pipeline that:
- Converts the store's product catalogue into searchable vectors
- Retrieves only the most relevant sections for each customer question
- Generates answers grounded in retrieved data (no hallucinations)
- Shows sources so customers trust the information

---

## Solution Overview

**Akbar & Sons Knowledge Assistant** is a single-page web app that combines:
- **Vector embeddings** (Google Gemini embeddings API)
- **Semantic search** (cosine similarity)
- **Grounded generation** (feed only relevant chunks + question to Gemini)
- **Source attribution** (every answer includes citations)

### How It Works

**1. Ingestion (One-time setup)**
```
Catalogue PDF → Extract text → Split into 27 semantic chunks → 
Embed each chunk (Gemini API) → Store vectors in browser cache
```

**2. Per-question retrieval**
```
User question → Embed question → Find top 3 similar chunks (cosine similarity) → 
Check confidence threshold → Generate answer from chunks OR return fallback
```

**3. Output**
```
Answer grounded in retrieved chunks + source citations (with match %)
```

---

## Key Features

✅ **Real Vector Embeddings** — Uses Google Gemini embedding model, not keyword search  
✅ **Multi-document Ingestion** — 27 built-in catalogue sections + user PDF uploads  
✅ **Semantic Search** — Finds relevant answers even with different wording  
✅ **Source Attribution** — Every answer shows which catalogue sections it came from  
✅ **Confidence Scoring** — Similarity percentages show how well answers match the question  
✅ **Fallback Handling** — Refuses to answer out-of-scope questions instead of guessing  
✅ **Browser Caching** — Vectors cached in localStorage; fast retrieval on subsequent questions  
✅ **Mobile-Responsive** — Works on phones, tablets, desktops  
✅ **No Backend Required** — All AI work happens in the browser (client-side only)  

---

## Tech Stack

| Layer | Technology |
|-------|------------|
| **Frontend** | HTML5, CSS3, JavaScript (ES6) |
| **Embeddings** | Google Gemini embedding-001 API |
| **Generation** | Google Gemini 2.5 Flash API |
| **Document Processing** | pdf.js (extract text from PDFs) |
| **Storage** | localStorage (vector cache) |
| **Retrieval** | Cosine similarity (vanilla JS) |
| **Deployment** | GitHub Pages (static hosting) |
| **Design System** | Navy (#0a1940) + Gold (#c9a84c), Cinzel + Raleway fonts |

---

## Getting Started

### Prerequisites
- A Google Gemini API key (free tier available)
- Modern browser (Chrome, Firefox, Safari, Edge)
- No backend/server setup required

### How to Use

1. **Open the app:** https://abubakar-aleem.github.io/Akbar-Sanitary-Retrieval-Augmented-Knowledge-Assistant---Innoviast/

2. **Enter API key:**
   - Get free key from [Google AI Studio](https://aistudio.google.com/app/apikey)
   - Paste into "Connect" section
   - Click "Save Key" (stored securely in browser only)

3. **Initialize knowledge base:**
   - Click "Initialize Knowledge Base"
   - Watch progress bar (embeds all 27 sections)
   - Takes ~30 seconds first time, then cached

4. **Ask questions:**
   - Type any question about products, brands, sizes, store hours, etc.
   - See answer with sources
   - Try the quick-question chips for examples

### Optional: Upload Custom PDFs
- Drag-and-drop or select a PDF in the "Build Knowledge Base" section
- System extracts text, chunks it, and adds to knowledge base
- Helpful for adding product updates or new catalogues

---

## Example Questions & Answers

**Q: Which brands sell PPRC pipes?**  
A: Popular, TurkPlast, Dura Flow, and IIL all sell PPRC pipes.  
Sources: PPRC Pipes & Fittings (77%), PPRC Heaters (77%)

**Q: What sizes do water tanks come in?**  
A: Our water tanks come in 150, 200, 250, 300, 400, 500, 750, and 1200 Gallon capacities.  
Sources: Water Tanks (73%)

**Q: Do you sell electrical wire?**  
A: We do not have information about electrical wire or cables in our catalogue. Please contact the store directly at 0321-1306560 for more details.  
Sources: (Fallback — no high-confidence match)

---

## Architecture

See `architecture-diagram.png` in the repo for a visual breakdown of:
- Document ingestion pipeline
- Embedding & vector storage
- Query → retrieval → generation flow
- Fallback mechanism

---

## Test Results

**5/5 test questions passed:**
- Brand retrieval: 100% accurate
- Multi-field extraction: 100% accurate
- Fallback handling: working correctly
- Cross-document comparison: accurate synthesis
- Store info retrieval: precise

See `EVALUATION_SHEET.pdf` for detailed test methodology and results.

---

## Knowledge Base

**Built-in catalogue (27 sections):**
- UPVC Pipes & Fittings
- PPRC Pipes & Fittings
- Sewerage Pipes & Fittings
- Water Tanks
- Valves (check, handle, no-return)
- Faucets & Mixers
- Sanitary Ware (commodes, basins, flushes)
- Kitchen Sinks
- Water Pumps & Motors
- Garden Pipes
- GI Pipes & Fittings
- Bathroom Showers
- Floor Waste / Drains
- Basin Accessories
- Water Filters
- Gas Pipes
- Adhesives & Silicones
- PPRC Heaters
- Curtain Pipes & Brackets
- Door Locks
- Thermopol Sheets
- Folding Ladders
- Threads & Tapes
- Store Information

Source: Akbar_and_Sons_Catalogue.pdf (included in repo)

---

## Learning Outcomes

✅ **Retrieval-Augmented Generation (RAG)** — understanding how to ground LLM output in real data  
✅ **Vector embeddings** — converting text to numeric representations for semantic search  
✅ **Similarity search** — using cosine similarity to find relevant documents  
✅ **Prompt engineering** — crafting system prompts that enforce grounding constraints  
✅ **Document ingestion** — extracting and chunking text from PDFs programmatically  
✅ **Fallback handling** — building systems that know what they don't know  
✅ **Browser APIs** — localStorage, localStorage, fetch, FormData for file uploads  
✅ **UI/UX for AI** — showing confidence scores, sources, and fallback messages clearly  

---

## Limitations & Future Work

**Current Limitations:**
- Vectors cached in browser (max ~50 documents practical limit)
- All processing happens in browser (slower for large corpora)
- No user history/analytics
- No multi-language support yet

**Future Improvements:**
- Backend vector database (Pinecone, Supabase) for scaling
- Fine-tuned embeddings for domain-specific terminology
- User feedback loop (upvote/downvote answers)
- Admin panel to update knowledge base without redeploying
- Real-time analytics (common questions, fallback rate)

---

## Deployment

Deployed live on GitHub Pages:
- **Live URL:** https://abubakar-aleem.github.io/Akbar-Sanitary-Retrieval-Augmented-Knowledge-Assistant---Innoviast/
- **Repo:** https://github.com/abubakar-aleem/Akbar-Sanitary-Retrieval-Augmented-Knowledge-Assistant---Innoviast
- **Branch:** `main`
- **Auto-deploy on push:** Yes

---

## Files in This Repo

```
├── index.html                          # Main app (single-file build)
├── README.md                           # This file
├── AI_USAGE.md                         # AI tools, APIs, prompts used
├── EVALUATION_SHEET.pdf                # Test results (5/5 pass)
├── Akbar_and_Sons_Catalogue.pdf        # Sample knowledge base document
├── architecture-diagram.png            # RAG pipeline visual
└── ss/                                 # Screenshots (optional)
    ├── 1.png - Landing page
    ├── 2.png - Initialize KB
    ├── 3.png - Question asked
    ├── 4.png - Answer with sources
    └── 5.png - Fallback example
```

---

## Store Info

**Akbar & Sons Sanitary Store**  
Owner: M Aleem  
Address: Main Defence Ghazi Road, near Jamia Masjid Siddiqia, Lahore  
Phone/WhatsApp: 0321-1306560  
Hours: 9:00 AM – 9:00 PM  
Services: Delivery available, bulk supply for contractors/builders

---

## How to Contribute

If you'd like to:
- **Add products** → Update `Akbar_and_Sons_Catalogue.pdf` and re-initialize
- **Improve prompts** → Edit the system prompt in `index.html` (search for "system prompt")
- **Adjust UI** → Modify CSS variables (navy, gold colors defined at top of `<style>`)
- **Report bugs** → Open an issue on GitHub

---

## License

Educational project, MIT License. Free to use, modify, distribute.

## Author

**Abubakar Aleem**  
Computer Science Student | Freelance Developer | Innoviast Intern  
GitHub: [@abubakar-aleem](https://github.com/abubakar-aleem)  
Project: Week 4 — AI Development Track, Innoviast IT Solutions & Services

Built Summer 2026 as part of the AI Chatbot Developer Internship.
