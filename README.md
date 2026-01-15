# n8n-ni8mare-educational

LEGAL DISCLAIMER

This project is for educational and research purposes only. The author does not endorse or encourage using this information for malicious purposes. This lab is designed to demonstrate a specific vulnerability.

Do not use this exploit against systems you do not own or have explicit permission to test.

## About CVE-2026-21858
This lab demonstrates a vulnerability within an AI-powered n8n workflow.

By manipulating the JSON payload sent to a Form Trigger, an attacker can trick the workflow into:
* Accepting a local server path (e.g., /etc/passwd) as if it were an uploaded file.
* Indexing the content of that sensitive file into the Vector Database.
* Retrieving the sensitive data via the AI Chat Interface.

---
## Prerequisites
* Docker and Docker Compose installed.
* An OpenAI API Key (for the AI Agent).
* curl (installed on your host machine to run the exploit script).

## Setup Instructions
1. Start the Environment
Spin up the vulnerable n8n instance pinned to the specific version for this lab.



```bash
docker compose up -d
```

Once the containers are running, navigate to:

n8n Dashboard: http://localhost:5678

## Import the Vulnerable Workflow
Open the n8n dashboard.

If this is your first time, create a local owner account.

Go to the Workflows menu and select "Import from File".

Select WorkflowForCve.json from this repository.


