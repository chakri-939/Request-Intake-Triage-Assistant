# 🤖 Automated Request Intake & Triage Assistant

An enterprise-grade, zero-cost automated workflow built for **Nederlands de Baas** to streamline partnership requests, eliminate manual administrative overhead, and automate intelligent task routing.

## 🔄 System Architecture & Data Flow

```text
[Google Form] ➡️ [Google Sheets] ➡️ [Self-Hosted n8n] ➡️ [Google Gemini 1.5 Flash API] ➡️ [monday.com Board Item] ➡️ [Gmail Notifications]
```
Workflow Video : https://youtu.be/ftjJiQj0--I
* **Intake:** External partners submit partnership or event inquiries via a custom Google Form. Responses automatically populate a linked Google Sheet.
* **Trigger & Extraction:** A row-added event triggers a self-hosted n8n automation instance.
* **AI-Powered Triage:** An HTTP Request node sends the payload to Google Gemini 1.5 Flash, which analyzes the request context, assigns an urgency priority (`Low`, `Medium`, `High`), categorizes the organization type, generates brief internal notes, and routes the task to a designated employee.
* **Data Normalization & Parsing:** A custom JavaScript node sanitizes raw LLM strings, strips markdown code blocks, parses clean JSON, and normalizes Google Sheet header whitespace.
* **Project Management Sync:** A monday.com API node dynamically creates a new board item mapped to the correct group (`To-Do`) and fills status, text, date, and assigned employee columns.
* **Bilingual Notifications:** The workflow forks into parallel Gmail nodes:
  - Sends an automated acknowledgment confirmation to the external requester.
  - Dispatches a priority internal alert containing AI triage summaries for the assigned employee.
 
## 📷 Workflow Canvas & Dashboard Preview

1. **n8n Automation Workflow Canvas**
2. **Live monday.com Triage Board**
3. **Intake Form**

---

## 💡 Technical Challenges Solved

- **Handling LLM Output Variability:** LLMs frequently wrap JSON responses in markdown code blocks (` ```json ... ``` `). Implemented robust string replacement and sanitization logic inside the n8n JavaScript node to ensure clean parsing without downstream workflow failures.
- **Header Normalization:** Addressed hidden whitespace padding in Google Sheet headers dynamically via a key-trimming loop, preventing expression-mapping lookup errors.
- **Structured API Payload Mapping:** Mapped complex nested properties (such as status label objects and multi-type board columns) for the monday.com backend.
