# CareerCoach AI

An AI-powered career assistant that analyzes how well your CV matches a job posting - and tells you exactly how to improve it.

Built as a learning project to explore n8n automation, AI agent workflows, and no-code frontend development with Lovable.

---

## 🔗 Link

🌐 Website: https://career-coach-ai-pp.lovable.app/ 

---

## What it does

1. The user uploads their CV (PDF/DOCX) or provides a Google Drive link
2. The user pastes a job posting URL (LinkedIn, dev.bg) or the job description as plain text
3. The backend scrapes the job posting, extracts the CV text, and sends both to an AI model
4. The AI returns a structured analysis including:
   - **Match Score** — how well the CV matches the job (0-100%)
   - **Potential Score** — what the match could be with improvements
   - **Strengths** — what's already working in the candidate's favor
   - **Critical Gaps** — what's missing for this specific role
   - **Improvements** — concrete actionable suggestions
   - **Recommendation** — Apply / Consider / Skip
   - **Reasoning** — a short narrative explaining the overall assessment
5. The user can view the full analysis on a dedicated results page

---

## Tech Stack

| Layer | Tool | Purpose |
|---|---|---|
| Frontend | Lovable.dev | UI — form, loading state, results page |
| Automation | n8n (Cloud) | Workflow orchestration |
| AI Model | OpenAI GPT-4o-mini | CV analysis and scoring |
| Web Scraping | Apify (cheerio-scraper) | Job posting content extraction |
| Job API | LinkedIn Guest API | LinkedIn job posting access (no auth) |
| CV Storage | Google Drive (public link) | Optional CV source |
| File Parsing | n8n Extract from File | PDF/DOCX text extraction |

---

## Architecture

### Frontend (Lovable)
- Desktop-first responsive UI
- Two CV input modes: file upload or Google Drive link
- Two job input modes: URL or plain text
- Loading state while waiting for analysis
- Dedicated results page with animated score rings
- Error handling for inaccessible files or unsupported URLs

### Backend (n8n Workflow)
The workflow runs in parallel - CV processing and job scraping happen simultaneously:

---

## Project Structure

```
careercoach-ai/
  ├── frontend/           # Link to Lovable repo (see below)
  ├── backend/
  │   ├── workflow.json   # n8n workflow export (importable)
  │   ├── screenshots/    # Visual overview of the n8n workflow
  │   └── README.md       # Backend setup instructions
  ├── docs/
  │   ├── process_flow.md          # System architecture diagram
  │   └── system_architecture.md   # User flow diagram
  └── README.md
```

---

## Repos

- **Frontend:** [https://github.com/plamenapavlova/career-coach-ai-pp]
- **Backend workflow:** `/backend/workflow.json` - import into n8n to replicate

---


## Supported Job Sources

| Source | Status |
|---|---|
| LinkedIn | ✅ Supported (LinkedIn Guest API) |
| dev.bg | ✅ Supported (Apify scraper) |
| Plain text | ✅ Supported (paste directly) |
| jobs.bg | ❌ Not supported (Cloudflare protection) |

---

## CV Input Options

| Method | Status |
|---|---|
| PDF upload | ✅ Supported |
| DOCX upload | ✅ Supported |
| Google Drive link | ✅ Supported (must be set to "Anyone with the link") |

---

## What I learned building this

- n8n workflow design — nodes, expressions, branching, merging parallel flows
- Prompt engineering for structured JSON output from LLMs
- Web scraping strategies and Cloudflare bypass limitations
- Webhook-based frontend/backend integration
- Binary file handling in automation workflows
- Building no-code frontends with Lovable and connecting them to a backend
- Error handling across distributed systems

---

## Status

🟡 **In progress** — core analysis flow working end-to-end. Planned next steps:
- CV rewrite feature
- Job text paste mode improvements
- History/saved analyses
- Support for more job sites

---

## Author

Plamena Pavlova
