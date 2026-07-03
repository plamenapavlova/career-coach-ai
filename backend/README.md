# Backend — n8n Workflow

## Overview

The backend is a single n8n workflow that:
1. Receives a POST request from the Lovable frontend via Webhook
2. Processes the CV (file upload or Google Drive link)
3. Fetches the job posting content (URL scraping or plain text)
4. Runs two sequential AI analyses via OpenAI GPT-4o-mini
5. Returns a structured JSON response to the frontend


## Workflow nodes overview

### CV Processing Branch
- **Webhook** — receives the incoming request
- **Switch CV format** — routes based on `cv_mode` (file or link)
- **PDF or Text file** — detects file type (PDF vs DOCX)
- **Extract from PDF** — extracts text from PDF binary
- **Extract from text** — extracts text from DOCX binary
- **Extract file ID from Drive** — parses Google Drive file ID from URL
- **Download CV** — downloads the file from Google Drive
- **Check for error** — validates binary exists after download
- **If no error** — validates extracted CV text is meaningful

### Job Processing Branch
- **Chekc Job desc format** — routes based on `job_mode` (url or text)
- **Switch Job URL type** — routes based on URL domain (linkedin / other)
- **LinkedIn Job ID** — extracts job ID from LinkedIn URL
- **Fetch LinkedIn job** — calls LinkedIn Guest API
- **Start Apify Scrape** — starts cheerio-scraper job for other URLs
- **Wait** — waits 15 seconds for Apify to finish
- **Fetch Scraped Content** — retrieves scraped text from Apify dataset
- **Access job url** — validates scraped content is meaningful
- **Set job text field** — passes plain text directly when text mode
- **Normalize Job Text** — normalizes job text from any source into consistent format

### AI Analysis
- **Merge** — waits for both CV and job text before proceeding
- **Match % + analysis** — first AI call: match score, strengths, gaps, improvements, recommendation
- **Validator + JSON parse** — parses and validates first AI response
- **Potential % + reasoning** — second AI call: potential score and reasoning narrative
- **Validator + JSON parse 2** — parses and validates second AI response
- **Combine result** — merges both AI outputs into one clean JSON object
- **Respond to Webhook** — returns final JSON to frontend

## Request format (from frontend)

### File upload (multipart/form-data)
```
cv          → binary file (PDF or DOCX)
cv_mode     → "file"
cv_url      → "" (empty)
job_mode    → "link" or "text"
job_url     → job posting URL (if link mode)
job_text    → "" (empty if link mode)
```

### Link mode (application/json)
```json
{
  "cv_mode": "link",
  "cv_url": "https://drive.google.com/...",
  "job_mode": "link",
  "job_url": "https://dev.bg/...",
  "job_text": ""
}
```

## Response format (to frontend)

### Success
```json
{
  "match_score": 75,
  "potential_score": 85,
  "strengths": ["...", "..."],
  "critical_gaps": ["...", "..."],
  "improvements": ["...", "..."],
  "recommendation": "Consider",
  "reasoning": "A short narrative explaining the overall assessment."
}
```

### Error
```json
{
  "error": true,
  "message": "Human-readable error message explaining what went wrong."
}
```

## Error handling

| Scenario | How it's handled |
|---|---|
| Google Drive link inaccessible | Check for error node → Return URL error message |
| CV text extraction failed | If no error node → Return CV file error |
| Job URL blocked/inaccessible | Access job url node → Return job URL error |
| jobs.bg URL | Blocked at frontend (Lovable validation) |
| LinkedIn URL | Routed to LinkedIn Guest API instead of Apify |
