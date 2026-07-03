**Frontend — CareerCoach AI**

A desktop-first single-page application built with React via Lovable.dev. It serves as the user-facing interface for the CareerCoach AI system.
Link: https://github.com/plamenapavlova/career-coach-ai-pp 

**Input Form**
- Two CV input modes: direct file upload (PDF/DOCX) or Google Drive shared link
- Two job posting input modes: URL (LinkedIn, dev.bg) or plain text paste
- Frontend validation before submission (missing CV, unsupported jobs.bg URLs, empty fields)
- Submit triggers a POST request to the n8n webhook with the CV and job data

**Loading State**
- Displays a thinking/progress animation while waiting for the webhook response
- Shows step-by-step processing messages to reduce perceived wait time
- Only activates after a successful webhook response, not before

**Results Page**
- Dedicated `/results` route rendered after successful analysis
- Animated circular score ring showing Match Score (0-100%)
- Secondary ring showing Potential Score with "+X possible" indicator
- Recommendation badge (Apply / Consider / Skip) color-coded by outcome
- Reasoning narrative card explaining the overall assessment
- Strengths, Critical Gaps, and Improvements sections with clear visual hierarchy
- "Run another analysis" button to return to the input form

**Error Handling**
- Inline error messages for inaccessible Google Drive links
- Inline error for unreadable CV files
- Inline error for inaccessible job posting URLs
- All errors stay on the form page without navigating away
