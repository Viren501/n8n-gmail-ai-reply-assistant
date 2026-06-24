# AI-Powered Inbox-to-Airtable Email Reply Assistant

An automated, self-contained **n8n workflow** that monitors your Gmail inbox for unread messages, leverages Google Gemini (`gemini-2.5-flash-lite`) to draft tailored professional replies, stages the responses in Airtable for review, and clears the inbox queue.

## 📁 Workflow Architecture

This workflow operates on a recurring execution model to seamlessly process inbound communications:

1. **Polling Trigger:** A cron-based **Schedule Trigger** fires automatically every 2 hours to check for new mail.
2. **Data Extraction:** The **Gmail** node fetches unread emails and pulls complete thread details (sender metadata, body content, and attachments).
3. **AI Triaging & Drafting:** A LangChain-backed **AI Agent** utilizing **Google Gemini** evaluates the inbound text. It applies custom logic (e.g., declining cold collaborations while welcoming project/meeting discussions) and outputs a raw, ready-to-send response body.
4. **Database Staging:** An **Airtable** record is created to store the Sender Name, Email, Original Body, Drafted Reply, and the unique Gmail Thread ID.
5. **Queue Cleanup:** The workflow references the thread context to **Mark the message as read**, finalizing the batch.

---

## 🛠️ Core Integration Nodes & Requirements

To successfully deploy this workflow, you must have an active n8n instance (v1.0+) featuring Advanced AI capabilities, alongside valid credentials for:

* **Gmail API:** OAuth2 setup configured to read/write messages and modify labels.
* **Google Gemini API:** Access credentials for the Google PaLM/Gemini node ecosystem.
* **Airtable API:** A Personal Access Token (PAT) with write permissions onto your operational Base and Table.

---

## 🚀 Installation & Deployment Guide

### 1. Import into your n8n Instance
1. Clone or download this repository locally.
2. Navigate to your n8n workspace dashboard.
3. Select **Workflows** -> **Add Workflow** to initialize a blank workspace canvas.
4. Click the options menu (`...`) in the upper right-hand corner and choose **Import from File**.
5. Upload the `workflows/inbox-to-airtable-reply-agent.json` file.

### 2. Configure Credentials and Identifiers
Because environment details are sanitized out of the source blueprint, configure the missing environment properties across these nodes:
* **Gmail All Mails / Details / Mark as Read:** Bind your verified Gmail OAuth2 account profile.
* **Google Gemini Chat Model:** Link your Gemini API token profile.
* **Airtable (Create a Record):** Select your unique target **Base ID** and target **Table ID** from the drop-down interfaces, mapping fields accordingly.

### 3. Take the Automation Live
Test the workflow manually once using the **Test step** command. If executions route smoothly into Airtable, turn the global activation switch in the top right header from **Inactive** to **Active**.
